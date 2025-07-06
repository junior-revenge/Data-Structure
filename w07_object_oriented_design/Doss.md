# 7.2 Call Center

Imagine you have a call center with three levels of employees: respondent, manager, and director. An incoming telephone call must be first allocated to a respondent who is free. If the respondent can't handle the call, he or she must escalate the call to a manager. If the manager is not free or not able to handle it, then the call should be escalated to a director. Design the classes and data structures for this problem. Implement a method dispatchCall() which assigns a call to the first available employee.

## Approach

The problem requires three different types of call handlers organized in a vertical hierarchy: respondent → manager → director. To address this, I suggest using a handler pool along with a common handler interface.

## Handler Classes & Interface

```java
interface ICallHandler {
    CallHandleResult    handleCall();
    boolean             isAvailable();
}
```

Every handler class must implement the ICallHandler interface to standardize the handler type. This interface defines two methods: handleCall(), which handles a call and returns the result, and isAvailable(), which indicates whether the handler is currently available.

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

Every call handler return the CallHandleResult class as a result.

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

Each call handler class maintains a status indicating whether it is currently handling a call. If a handler is busy, the isAvailable() method returns false.

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

The class above represents a call handler pool, which manages a specific group of call handlers. It maintains a collection of handlers and returns an available handler instance when requested. Additionally, it holds a reference to the next-level handler pool for escalation.

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

The class above handles incoming call requests and dispatches them to the appropriate handler. It maintains three different call handler pools—responder, manager, and director—arranged in a hierarchical order. When a call is dispatched, the class first attempts to assign it to the responder pool. If no responder is available or fail to handle a call, it escalates the call to the next pool in the chain, continuing until it either finds an available handler or reaches the final pool.