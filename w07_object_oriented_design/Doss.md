# 7.2 Call Center

Imagine you have a call center with three levels of employees: respondent, manager, and director. An incoming telephone call must be first allocated to a respondent who is free. If the respondent can't handle the call, he or she must escalate the call to a manager. If the manager is not free or not able to handle it, then the call should be escalated to a director. Design the classes and data structures for this problem. Implement a method dispatchCall() which assigns a call to the first available employee.

## Approach

The problem requires 3 different call handler and they must have vertical heirachical structure, respondent -> manager -> director. I suggest to use handler pool and handler interface for this problem.

## Handler Classes & Interface

```java
interface ICallHandler {
    CallHandleResult    handleCall();
    boolean             isAvailable();
}
```

Every handler class must inherit and implement ICallHandler interface to generalize handler type.

```java
class CallHandleResult {
    private boolean success;
    private String message;

    public CallHandleResult(boolean success, String message) {
        this.success = success;
        this.message = message;
    }

    public boolean isSuccess() {
        return success;
    }

    public String getMessage() {
        return message;
    }
} // err, result returnning structure
```

Every call handler return the result class wheather it failed or not.

## Handler Classes

```java
class Responder implements ICallHandler {
    private boolean handlingCall;
    
    @Override
    public CallHandleResult handleCall() {
        return new CallHandleResult(false, "I can't handle this call.");
    }

    @Override
    public boolean isAvailable() { return !handlingCall; }
}

class Manager implements ICallHandler {
    private boolean handlingCall;
    
    @Override
    public CallHandleResult handleCall() {
        return new CallHandleResult(false, "Please take over to the director.");
    }

    @Override
    public boolean isAvailable() { return !handlingCall; }
}

class Director implements ICallHandler {
    private boolean handlingCall;
    
    @Override
    public CallHandleResult handleCall() {
        return new CallHandleResult(true, "Director can handle all calls.");
    }

    @Override
    public boolean isAvailable() { return !handlingCall; }
}
```

## Call Handler Pool

```java
class CallHandlerPool  {
    private List<ICallHandler> handlers;
    private CallHandlerPool nextPool;

    public CallHandlerPool() {
        this.handlers = new ArrayList<ICallHandler>();
    }

    public ICallHandler getAvailableHandler() {
        for (ICallHandler handler : handlers) {
            if (handler.isAvailable()) {
                return handler;
            }
        }
        
        return null; // No available handler
    }

    public void addHandler(ICallHandler handler) {
        handlers.add(handler);
    }

    public void setNextPool(CallHandlerPool nextPool) {
        this.nextPool = nextPool;
    }

    public CallHandlerPool getNextPool() {
        return nextPool;
    }
}
```

## Call Center Class

```java
class CallCenter {
    private CallHandlerPool responderPool;
    private CallHandlerPool managerPool;
    private CallHandlerPool directorPool;

    public CallCenter() {
        responderPool = new CallHandlerPool();
        managerPool = new CallHandlerPool();
        directorPool = new CallHandlerPool();

        // Initialize pools with handlers
        responderPool.addHandler(new Responder());
        managerPool.addHandler(new Manager());
        directorPool.addHandler(new Director());

        // Set up the chain of responsibility
        responderPool.setNextPool(managerPool);
        managerPool.setNextPool(directorPool);
    }

    public void dispatchCall() {
        CallHandlerPool currentPool = responderPool;

        while (currentPool != null) {
            var result = currentPool.getAvailableHandler()
                .handleCall();

            if(result.isSuccess()) {
                System.out.println("Call handled successfully: " + result.getMessage());
                return;
            } else {
                System.out.println("Call not handled, " + result.getMessage());
                currentPool = currentPool.getNextPool();
            }
        }
    }
}
```