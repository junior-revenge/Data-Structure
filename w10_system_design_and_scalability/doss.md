# 9.3 Web Crawler

If you were designing a web crawler, how would you avoid getting into infinite loops?

## Crawler Code

```python
def crawl_site(root_url):
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
            # 같은 도메인인지 확인
            if urlparse(link).netloc == domain and link not in visited:
                queue.append((link, depth + 1))

        time.sleep(1)  # 서버 부하 방지

    return visited
```