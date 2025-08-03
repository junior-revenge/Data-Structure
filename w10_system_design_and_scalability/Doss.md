# 9.3 Web Crawler

If you were designing a web crawler, how would you avoid getting into infinite loops?

## What causes infinite loops in web crawling?

The purpose of a crawler is to collect web pages. Usually, it operates through the following process:

1. Fetch domains

2. Start crawling from the main page of a domain

3. Read the page

4. Queue the links found on the page

5. Repeat from step 3

### Circular Links (Self Links)

One common cause of infinite loops is circular linking, where pages link back to each other. This can trap the crawler in an endless cycle. The simplest solution is to keep track of visited pages using a data structure like a set.

### External Domains

Many websites include links to external domains. These don’t always cause infinite loops, but they can lead to excessive crawling or wasted resources. It’s advisable to check the domain of each link and decide whether to follow it.

### URL Parameters

Another typical case is pages with parameters, such as pagination (e.g., ?page=2). Forum-style websites often use page parameters, which can lead to deep or infinite crawling. A naive approach is to block URLs with parameters entirely.

However, this would prevent the crawler from indexing useful content, such as multiple pages of discussions on sites like Reddit or Medium. A better approach is to implement a depth limit, restricting how far the crawler follows parameterized links.

## Crawler Code

```python
def crawl_site(root_url, depth):
    visited = set()
    queue = [(root_url, 0)]
    headers = {"User-Agent": "Mozilla/5.0"}
    domain = urlparse(root_url).netloc

    while queue:
        url, depth = queue.pop(0)
        
        if url in visited:
            continue

        try:
            res = requests.get(url, headers=headers, timeout=5)
            if res.status_code != 200:
                continue
        except requests.RequestException:
            continue

        visited.add(url)
        print(f"[{len(visited)}] {url}")

        soup = BeautifulSoup(res.text, "html.parser")

        for link_tag in soup.find_all("a", href=True):
            link = urljoin(url, link_tag["href"])
            
            # page collecting and do something
            # ...
            # ...
            
            if depth > MAX_DEPTH:
                continue

            # Domain check
            if urlparse(link).netloc == domain and link not in visited:
                queue.append((link, depth + 1))

        time.sleep(1)  # Prevent server overloading

    return visited
```