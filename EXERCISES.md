# DevOps Training - Bộ bài tập thực hành

Bộ bài tập được thiết kế dựa trên dàn ý của tác giả trong các module README. Mỗi module có 3 cấp độ:

- **Cấp 1 - Cơ bản (Basic)**: Làm đúng theo hướng dẫn, hiểu khái niệm.
- **Cấp 2 - Vận dụng (Applied)**: Kết hợp nhiều khái niệm, giải quyết tình huống thực tế.
- **Cấp 3 - Vận dụng cao (Advanced)**: Troubleshoot, tối ưu, thiết kế giải pháp nâng cao.

Tiến độ theo dõi: `[ ]` chưa làm → `[x]` xong. Sau mỗi bài, ghi note/lesson learned vào phần "Ghi chú của tôi".

---

## Module 1: Preparing project

### Cấp 1 - Cơ bản

- [ ] **1.1** Chọn 1 ý tưởng sản phẩm thật (không phải todo app) có ít nhất 5 chức năng chính. Viết 1 đoạn pitch 100 từ mô tả: sản phẩm làm gì, cho ai, giải quyết vấn đề gì.
- [ ] **1.2** Tạo 2 repository riêng biệt trên GitHub: `<project>-backend` và `<project>-frontend`. Mỗi repo có `README.md`, `.gitignore` phù hợp, `LICENSE`.
- [ ] **1.3** Vẽ sơ đồ kiến trúc 3 tầng (FE - BE - DB) bằng [draw.io](https://draw.io) hoặc [Excalidraw](https://excalidraw.com/). Lưu vào `/docs/architecture.png`.
- [ ] **1.4** Viết tài liệu schema database (bảng, field, quan hệ) vào `/docs/database.md`.
- [ ] **1.5** Setup BE chạy ở `localhost:3000`, FE chạy ở `localhost:8080`. Chạy cả 2 song song không xung đột.

### Cấp 2 - Vận dụng

- [ ] **1.6** Áp dụng Gitflow: tạo branches `main`, `dev`, làm 1 feature trên branch `feature/xxx`, merge vào `dev` qua Pull Request. Viết commit message theo [Conventional Commits](https://www.conventionalcommits.org/).
- [ ] **1.7** Kiểm tra 12-Factor App checklist cho project của bạn. Với mỗi factor, ghi lại: đã đạt chưa, nếu chưa thì cần sửa gì. Lưu thành `/docs/12factor-review.md`.
- [ ] **1.8** Tạo file `.env.example` cho BE và FE, liệt kê đầy đủ biến môi trường cần thiết (không có giá trị thật). Đảm bảo `.env` thật nằm trong `.gitignore`.
- [ ] **1.9** Viết API documentation sơ bộ bằng [OpenAPI/Swagger](https://swagger.io/specification/) cho ít nhất 3 endpoints quan trọng.
- [ ] **1.10** Thiết kế mockup UI cho 3 màn hình chính bằng Figma hoặc Penpot. Export PNG/PDF vào `/docs/mockups/`.

### Cấp 3 - Vận dụng cao

- [ ] **1.11** Xây dựng ADR (Architecture Decision Record) giải thích tại sao chọn stack BE/FE/DB hiện tại. Liệt kê ít nhất 2 lựa chọn đã cân nhắc và lý do loại bỏ.
- [ ] **1.12** Giả sử dự án cần scale để phục vụ 10,000 users đồng thời. Viết tài liệu `/docs/scalability-plan.md` mô tả những điểm nào trong kiến trúc hiện tại sẽ trở thành bottleneck và giải pháp.
- [ ] **1.13** Thiết kế kiến trúc mở rộng thêm 1 dependency (ví dụ: Redis cache, message queue như RabbitMQ, hoặc search engine Elasticsearch). Cập nhật lại sơ đồ kiến trúc và giải thích khi nào dùng đến.
- [ ] **1.14** Kiểm tra `git log --all --full-history -- .env` xem có ai vô tình commit secrets không. Nếu có, dùng `git filter-repo` hoặc [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) để xoá sạch history.

### Ghi chú của tôi
```

```

---

## Module 2: Setup a server

### Cấp 1 - Cơ bản

- [ ] **2.1** Tạo 1 VPS Ubuntu LTS (22.04 hoặc 24.04) trên 1 nhà cung cấp free-tier (AWS EC2, Oracle Cloud, GCP, hoặc Azure). Ghi lại public IP.
- [ ] **2.2** SSH vào server bằng user mặc định (`ubuntu`, `root`,...). Chạy `uname -a`, `lsb_release -a`, `df -h`, `free -h` và note kết quả.
- [ ] **2.3** Tạo user mới có sudo (`adduser devops && usermod -aG sudo devops`). Test `sudo whoami`.
- [ ] **2.4** Setup SSH key-based auth cho user mới (copy public key vào `~/.ssh/authorized_keys`). Test login từ máy local không cần password.
- [ ] **2.5** Sửa `/etc/ssh/sshd_config`: `PermitRootLogin no`, `PasswordAuthentication no`, đổi `Port 22` sang 1 port custom (vd: 2222). Reload SSH và test.
- [ ] **2.6** Cài `ufw`, mở port SSH custom + 80 + 443, enable firewall. Test `telnet <ip> 2222` từ máy local.

### Cấp 2 - Vận dụng

- [ ] **2.7** Viết 1 script bash `harden.sh` tự động hoá các bước 2.3-2.6. Script phải idempotent (chạy 2 lần không phá server).
- [ ] **2.8** Cấu hình [Fail2ban](https://github.com/fail2ban/fail2ban) để auto-ban IP brute force SSH sau 3 lần sai. Test bằng cách cố tình login sai từ máy khác.
- [ ] **2.9** Setup `unattended-upgrades` cho auto-update security patches. Verify log tại `/var/log/unattended-upgrades/`.
- [ ] **2.10** Setup swap file 2GB (nếu RAM < 2GB) với đúng `swappiness` (10-20 cho production).
- [ ] **2.11** Tạo 2 users khác nhau (`dev1`, `dev2`) với quyền khác nhau. Thử nghiệm `sudo -l` xem ai làm được gì.
- [ ] **2.12** Dùng `cloud-init` / `user-data` để tự động tạo server đã hardened ngay khi boot lần đầu, thay vì SSH vào làm thủ công.

### Cấp 3 - Vận dụng cao

- [ ] **2.13** Chạy [Lynis](https://github.com/CISOfy/lynis) audit: `sudo lynis audit system`. Fix ít nhất 10 warning quan trọng nhất. Chạy lại và so sánh điểm hardening score.
- [ ] **2.14** Setup [auditd](https://man7.org/linux/man-pages/man8/auditd.8.html) để log lại mọi sudo command, việc thay đổi `/etc/passwd`, `/etc/ssh/sshd_config`. Xem log bằng `ausearch`.
- [ ] **2.15** Setup CrowdSec hoặc OSSEC như 1 lựa chọn thay cho Fail2ban. So sánh 3 công cụ, chọn 1 và viết tài liệu vì sao.
- [ ] **2.16** Thiết kế 1 runbook "Server bị compromise thì làm gì?" — các bước incident response (isolate, collect logs, forensic, recover).
- [ ] **2.17** Viết Ansible playbook tái tạo server đã hardened từ con số 0 (thay cho bash script ở 2.7). Test trên 1 VPS mới toanh.

### Ghi chú của tôi
```

```

---

## Module 3: Setup dependencies

### Cấp 1 - Cơ bản

- [ ] **3.1** Cài đặt database phù hợp project (PostgreSQL/MySQL/MongoDB) bằng package manager. Ghi lại version.
- [ ] **3.2** Tìm và document đường dẫn config file và data folder của database. Ví dụ với PostgreSQL: `/etc/postgresql/16/main/postgresql.conf`, `/var/lib/postgresql/16/main/`.
- [ ] **3.3** Đổi port mặc định (vd: PostgreSQL 5432 → 54321). Reload service và verify.
- [ ] **3.4** Tạo database mới + user mới với password mạnh. Grant quyền chỉ trên database đó.
- [ ] **3.5** Cấu hình database chỉ listen trên `127.0.0.1` hoặc LAN IP, KHÔNG listen trên `0.0.0.0`.
- [ ] **3.6** Block port database trên `ufw`. Test từ máy ngoài: `telnet <server-ip> 54321` phải fail.
- [ ] **3.7** Kết nối từ máy local qua SSH tunnel: `ssh -L 54321:127.0.0.1:54321 devops@<server-ip>`.

### Cấp 2 - Vận dụng

- [ ] **3.8** Tweak config cho database dựa trên RAM server. Ví dụ PostgreSQL dùng [PGTune](https://pgtune.leopard.in.ua/) để tính `shared_buffers`, `work_mem`,...
- [ ] **3.9** Setup backup tự động hằng ngày bằng cron: `pg_dump`/`mysqldump`/`mongodump` + nén + rotate (giữ 7 ngày gần nhất).
- [ ] **3.10** Di chuyển data folder sang 1 disk/partition khác (giả lập tình huống disk full). Update config, test application vẫn chạy.
- [ ] **3.11** Thêm 1 dependency thứ 2: Redis cho cache hoặc RabbitMQ cho queue. Apply cùng bộ rule: hardening + firewall + user + password.
- [ ] **3.12** Tạo read-only user riêng cho việc đọc logs/metrics, khác với user app dùng để ghi.

### Cấp 3 - Vận dụng cao

- [ ] **3.13** Setup streaming replication (PostgreSQL) hoặc replica set (MongoDB) giữa 2 server. Test failover: kill primary, verify app vẫn chạy qua replica.
- [ ] **3.14** Test disaster recovery: xoá hẳn database, restore từ backup gần nhất. Đo thời gian RTO (Recovery Time Objective).
- [ ] **3.15** Setup [WAL-G](https://github.com/wal-g/wal-g) hoặc [Barman](https://pgbarman.org/) cho Point-in-Time Recovery với PostgreSQL. Test recover về 1 thời điểm trong quá khứ.
- [ ] **3.16** Benchmark database bằng `pgbench`/`sysbench`. Tweak config, chạy lại, so sánh TPS (transactions per second).
- [ ] **3.17** Setup mã hoá at-rest cho data folder (dùng LUKS hoặc filesystem-level encryption). Đo impact về performance.
- [ ] **3.18** Thiết kế [database security cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html) riêng cho project của bạn, check từng mục đã làm chưa.

### Ghi chú của tôi
```

```

---

## Module 4: Deploy backend

### Cấp 1 - Cơ bản

- [ ] **4.1** Cài Node.js LTS bằng package manager (NodeSource) hoặc `nvm`. Verify `node -v`, `npm -v`.
- [ ] **4.2** Copy source code BE lên server bằng `rsync -avz --exclude='node_modules' --exclude='.git' --exclude='.env' ./ devops@<ip>:/home/devops/app/`.
- [ ] **4.3** Tạo file `.env` trên server từ `.env.example`, điền đúng connection string database local.
- [ ] **4.4** `npm ci --production` (dùng `ci` thay vì `install` để đảm bảo lock file).
- [ ] **4.5** Chạy `node index.js`, dùng `curl http://localhost:3000/health` để verify.
- [ ] **4.6** Cài pm2, chạy app với `pm2 start ecosystem.config.js`, setup `pm2 startup` để tự khởi động sau reboot.

### Cấp 2 - Vận dụng

- [ ] **4.7** Viết file `ecosystem.config.js` declarative: name, script, instances, env, log paths. KHÔNG truyền params qua CLI.
- [ ] **4.8** Bind app vào `127.0.0.1:3000` (không phải `0.0.0.0`). Hiểu lý do: reverse proxy sẽ expose ra ngoài, app chỉ cần nội bộ.
- [ ] **4.9** Setup log rotation với `pm2-logrotate` module hoặc system `logrotate`. Giới hạn 100MB/file, giữ 7 file.
- [ ] **4.10** Dùng `.nvmrc` để pin đúng Node version. Script deploy phải đọc `.nvmrc` và switch version trước khi build.
- [ ] **4.11** Setup health check endpoint `/health` trả về status của database connection, memory usage, uptime. Dùng pm2 `max_memory_restart` để restart khi rò rỉ memory.
- [ ] **4.12** Reboot server, verify app tự khởi động lại, DB vẫn connect được.

### Cấp 3 - Vận dụng cao

- [ ] **4.13** Chạy app ở cluster mode với `instances: 'max'` trong pm2. Verify thử bằng `htop`, xem nhiều process chạy song song.
- [ ] **4.14** Setup graceful shutdown: khi pm2 gửi SIGINT, app phải finish các request đang xử lý rồi mới tắt (dùng `server.close()` trong Node).
- [ ] **4.15** So sánh performance của 3 cách deploy: plain `node`, `pm2 fork mode`, `pm2 cluster mode`. Dùng `wrk` hoặc `k6` benchmark.
- [ ] **4.16** Tích hợp [APM tool](https://github.com/open-telemetry/opentelemetry-js) (OpenTelemetry, Elastic APM, hoặc New Relic free tier) để trace request.
- [ ] **4.17** Viết script `deploy.sh` rollback-able: mỗi lần deploy lưu vào `releases/<timestamp>/`, symlink `current → releases/xxx`. Rollback chỉ cần đổi symlink (Capistrano-style).
- [ ] **4.18** Chạy app dưới systemd service thay vì pm2. Viết file `/etc/systemd/system/myapp.service` hoàn chỉnh. So sánh ưu/nhược điểm 2 cách.

### Ghi chú của tôi
```

```

---

## Module 5: Deploy frontend

### Cấp 1 - Cơ bản

- [ ] **5.1** Verify Node version phù hợp FE (dùng nvm switch nếu khác BE).
- [ ] **5.2** Tạo Deploy Key (read-only) trên GitHub/GitLab, add public key vào repo. Clone repo bằng SSH URL lên server.
- [ ] **5.3** Tạo `.env` FE với `NEXT_PUBLIC_API_URL`, `VITE_API_URL`,... tương ứng framework.
- [ ] **5.4** `npm ci && npm run build`. Verify thư mục `dist/`/`build/`/`.next/` được tạo.
- [ ] **5.5** Với SPA static (CRA/Vite): dùng Nginx (tạm thời) hoặc `serve` package chạy ở port 8080.
- [ ] **5.6** Với SSR (Next.js/Nuxt): dùng pm2 start `npm start` ở port 8080.
- [ ] **5.7** Mở port 8080 trên UFW tạm thời, truy cập `http://<server-ip>:8080`, verify FE gọi được BE.

### Cấp 2 - Vận dụng

- [ ] **5.8** Fix lỗi CORS: hiểu vì sao FE gọi BE qua `http://<ip>:3000` từ browser thì CORS, còn từ localhost dev không CORS. Cấu hình BE cho phép đúng origin FE.
- [ ] **5.9** Hiểu sự khác biệt server-side fetch vs client-side fetch trong Next.js/Nuxt. Log cả 2 trường hợp, xem API URL phù hợp nào được dùng.
- [ ] **5.10** Tối ưu build: enable gzip/brotli pre-compression, minify, source map tách file riêng (không commit vào production).
- [ ] **5.11** Setup caching headers cho static assets (`/static/*` cache 1 năm, HTML cache 0 giây). Test bằng DevTools Network tab.
- [ ] **5.12** Build FE với env production KHÁC env dev. Verify biến env đã bake vào bundle, không lộ secrets FE không cần.

### Cấp 3 - Vận dụng cao

- [ ] **5.13** Setup [Bundle Analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) (Next/Vite equivalent), tìm 3 dependency lớn nhất, tìm cách giảm (tree-shaking, dynamic import, lazy load).
- [ ] **5.14** Setup service worker/PWA cho FE, test offline mode.
- [ ] **5.15** Với SSR: config [ISR (Incremental Static Regeneration)](https://nextjs.org/docs/app/guides/incremental-static-regeneration) hoặc equivalent. Đo TTFB so với pure SSR.
- [ ] **5.16** Deploy FE bản giống staging + production song song (2 port khác nhau), để test feature trước khi release.
- [ ] **5.17** Lighthouse audit: đạt 90+ ở cả 4 tiêu chí (Performance, Accessibility, Best Practices, SEO). Fix các issue được đề xuất.
- [ ] **5.18** Setup [CSP (Content Security Policy)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) nghiêm ngặt cho FE. Dùng report-only mode trước để không phá sập app.

### Ghi chú của tôi
```

```

---

## Module 6: Setup a domain

### Cấp 1 - Cơ bản

- [ ] **6.1** Mua 1 domain giá rẻ (vd `.xyz`, `.site` chỉ $1-2/năm) hoặc dùng domain miễn phí (DuckDNS, No-IP cho bài tập).
- [ ] **6.2** Tạo account Cloudflare free, add domain.
- [ ] **6.3** Đổi Nameserver ở registrar sang Cloudflare NS. Chờ propagation, verify bằng `dig NS yourdomain.com` hoặc [whatsmydns.net](https://whatsmydns.net).
- [ ] **6.4** Trên Cloudflare, tạo A record `yourdomain.com → <server-ip>` với Proxy mode **OFF** (DNS only, đám mây xám).
- [ ] **6.5** Truy cập `http://yourdomain.com:8080` → phải thấy FE.
- [ ] **6.6** Tạo thêm CNAME `www.yourdomain.com → yourdomain.com` với DNS only.

### Cấp 2 - Vận dụng

- [ ] **6.7** Tạo subdomain `api.yourdomain.com` A record → cùng server IP. Test từ `curl api.yourdomain.com:3000/health` (sẽ gọi BE).
- [ ] **6.8** Tạo thêm `staging.yourdomain.com` cho môi trường staging khác IP (nếu có server thứ 2, hoặc cùng IP khác port).
- [ ] **6.9** Dùng `dig +trace yourdomain.com` để hiểu DNS resolution qua root → TLD → authoritative.
- [ ] **6.10** Setup DNSSEC cho domain qua Cloudflare (enable ở Cloudflare, copy DS record về registrar).
- [ ] **6.11** Setup email records cơ bản: SPF, DKIM, DMARC (ngay cả khi chưa dùng email, để tránh domain bị abuse gửi spam).
- [ ] **6.12** Bật Proxy mode (cam) cho `yourdomain.com`, so sánh `dig` kết quả: thấy IP của Cloudflare chứ không phải IP server. Test app vẫn chạy được (port 80 qua Cloudflare).

### Cấp 3 - Vận dụng cao

- [ ] **6.13** Setup Cloudflare Firewall Rule: block toàn bộ truy cập từ các quốc gia bạn không phục vụ. Test bằng VPN.
- [ ] **6.14** Setup Page Rule / Cache Rule trên Cloudflare: cache static assets tại edge 1 tháng, bypass cache cho `/api/*`.
- [ ] **6.15** Di chuyển DNS sang 1 nhà cung cấp khác (vd AWS Route 53, DigitalOcean DNS). Lập plan migration zero-downtime: giảm TTL, migrate records, đổi NS, monitor.
- [ ] **6.16** Hiểu và thiết lập [GeoDNS / Latency-based routing](https://en.wikipedia.org/wiki/Geolocation_software) (yêu cầu cloud DNS, vd Route 53). Giả lập 2 server ở 2 region khác nhau.
- [ ] **6.17** Đăng ký thêm 1 top-level domain khác (vd `.dev`, `.io`), setup redirect `www` + apex → canonical domain.

### Ghi chú của tôi
```

```

---

## Module 7: Setup a reverse proxy

### Cấp 1 - Cơ bản

- [ ] **7.1** Cài Nginx: `sudo apt install nginx`. Verify `curl http://localhost` thấy trang "Welcome to nginx".
- [ ] **7.2** Tắt default site: `sudo rm /etc/nginx/sites-enabled/default`.
- [ ] **7.3** Cấu hình Nginx trả về 444 (no response) cho request không match domain đã setup: thêm `server { listen 80 default_server; return 444; }`.
- [ ] **7.4** Tạo `/etc/nginx/sites-available/yourdomain.com` với 2 location: `/` proxy vào `http://127.0.0.1:8080`, `/api/` proxy vào `http://127.0.0.1:3000/` (strip `/api` prefix bằng `proxy_pass` kết thúc có dấu `/`).
- [ ] **7.5** Symlink `sites-available/yourdomain.com → sites-enabled/`. `nginx -t && sudo systemctl reload nginx`.
- [ ] **7.6** Đóng port 3000, 8080 trên UFW (không cần public nữa). Chỉ mở 80, 443.
- [ ] **7.7** Truy cập `http://yourdomain.com` → FE. `http://yourdomain.com/api/health` → BE.

### Cấp 2 - Vận dụng

- [ ] **7.8** Hiểu sự khác biệt giữa 4 loại location matching: `=`, `^~`, `~`, `~*`, regex vs prefix. Viết 1 config có ít nhất 3 trong 4 loại.
- [ ] **7.9** Hiểu sự khác biệt `proxy_pass http://upstream;` và `proxy_pass http://upstream/;` (có/không trailing slash). Test 2 trường hợp và log URL BE nhận được.
- [ ] **7.10** Setup 2 server block: `yourdomain.com` cho FE, `api.yourdomain.com` cho BE. Sửa FE gọi `https://api.yourdomain.com` thay cho `/api/`.
- [ ] **7.11** Gặp và giải quyết CORS khi dùng 2 domain (7.10). KHÔNG dùng `*`. Config Nginx hoặc BE chỉ cho phép `yourdomain.com`.
- [ ] **7.12** Forward đúng header: `proxy_set_header Host $host; X-Real-IP $remote_addr; X-Forwarded-For $proxy_add_x_forwarded_for; X-Forwarded-Proto $scheme;`. Verify BE log thấy IP thật của client.
- [ ] **7.13** Setup `client_max_body_size 20M` để upload file lớn. Test bằng `curl -F "file=@big.jpg"`.
- [ ] **7.14** Setup WebSocket proxy (nếu BE có dùng): `proxy_http_version 1.1; proxy_set_header Upgrade $http_upgrade; proxy_set_header Connection "upgrade";`.

### Cấp 3 - Vận dụng cao

- [ ] **7.15** Setup rate limiting: `limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;` cho location `/api/`. Test bằng `ab` hoặc `hey`.
- [ ] **7.16** Setup upstream với load balancing: 2-3 instance BE chạy ở 3000, 3001, 3002. Cấu hình `upstream backend` với round-robin, least_conn, health check.
- [ ] **7.17** Setup Nginx cache cho các GET request của BE (vd `/api/products`) với `proxy_cache_path`. Tuning `proxy_cache_valid`, `proxy_cache_use_stale`.
- [ ] **7.18** Security hardening: ẩn version (`server_tokens off`), giới hạn methods (`if ($request_method !~ ^(GET|POST|PUT|DELETE)$ ) { return 405; }`), block user-agent xấu, block `.git`, `.env` paths.
- [ ] **7.19** Setup response headers: `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`, `Referrer-Policy`, `Permissions-Policy`. Check điểm bằng [securityheaders.com](https://securityheaders.com).
- [ ] **7.20** Viết 1 Nginx module/location riêng log detailed truy cập vào 1 file khác (vd log JSON format để sau đẩy vào ELK).
- [ ] **7.21** Setup `nginx-amplify` hoặc [lua-resty-waf](https://github.com/p0pr0ck5/lua-resty-waf) / [ModSecurity](https://github.com/owasp-modsecurity/ModSecurity-nginx) làm WAF basic.
- [ ] **7.22** Thay Nginx bằng Caddy / Traefik cho cùng 1 scenario. So sánh 3 reverse proxy và viết bảng so sánh pros/cons.

### Ghi chú của tôi
```

```

---

## Module 8: Setup SSL

### Cấp 1 - Cơ bản

- [ ] **8.1** Đọc [How HTTPS works](https://howhttps.works/) và viết 1 đoạn 200 từ giải thích handshake TLS 1.3 bằng từ của mình.
- [ ] **8.2** Phân biệt được: trusted CA cert vs self-signed cert, HTTP-01 challenge vs DNS-01 challenge, RSA vs ECDSA cert.
- [ ] **8.3** Cài `certbot` với plugin nginx: `sudo apt install certbot python3-certbot-nginx`.
- [ ] **8.4** Lấy cert cho `yourdomain.com` và `www.yourdomain.com` bằng HTTP-01: `sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com`.
- [ ] **8.5** Verify certbot đã tự sửa Nginx config để listen 443 + redirect 80 → 443. Test `https://yourdomain.com` có lock xanh.
- [ ] **8.6** Test `sudo certbot renew --dry-run`. Verify cron job `/etc/cron.d/certbot` hoặc systemd timer `certbot.timer` active.

### Cấp 2 - Vận dụng

- [ ] **8.7** Viết lại Nginx config SSL bằng tay (không dùng certbot `--nginx`): listen 443, `ssl_certificate`, `ssl_certificate_key`, redirect 80 → 443 bằng `return 301`.
- [ ] **8.8** Đảm bảo Nginx vẫn serve được `/.well-known/acme-challenge/` qua HTTP 80 khi renew. Test bằng `certbot certonly --webroot`.
- [ ] **8.9** Lấy cert cho cả `api.yourdomain.com` bằng cách thêm `-d api.yourdomain.com` (SAN cert). HOẶC lấy cert riêng.
- [ ] **8.10** Tối ưu SSL config theo [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/) - chế độ `intermediate`. Test điểm ở [SSL Labs](https://www.ssllabs.com/ssltest/) phải đạt A hoặc A+.
- [ ] **8.11** Enable HSTS (`Strict-Transport-Security: max-age=31536000; includeSubDomains`). Hiểu hậu quả nếu bật HSTS rồi muốn quay lại HTTP.
- [ ] **8.12** Enable OCSP stapling (`ssl_stapling on; ssl_stapling_verify on;`). Test bằng `openssl s_client -connect yourdomain.com:443 -status`.

### Cấp 3 - Vận dụng cao

- [ ] **8.13** Lấy wildcard cert `*.yourdomain.com` bằng DNS-01 challenge: setup Cloudflare API token, dùng plugin `certbot-dns-cloudflare`. Test renew tự động.
- [ ] **8.14** Đổi certbot sang [acme.sh](https://github.com/acmesh-official/acme.sh). Thử với 1 CA khác (ZeroSSL, Buypass). Compare workflow.
- [ ] **8.15** Setup monitoring cert expiry: script kiểm tra `openssl x509 -enddate`, push metric vào [Uptime Kuma](https://github.com/louislam/uptime-kuma) hoặc Grafana. Alert khi cert còn < 20 ngày.
- [ ] **8.16** Setup Certificate Transparency log monitoring: dùng [Cert Spotter](https://sslmate.com/certspotter/) để biết khi có ai issue cert bất hợp pháp cho domain bạn.
- [ ] **8.17** Cài đặt mTLS (mutual TLS) cho `api.yourdomain.com`: client phải present cert thì BE mới accept. Test bằng `curl --cert client.crt --key client.key`.
- [ ] **8.18** Setup private CA bằng [step-ca](https://smallstep.com/docs/step-ca/) hoặc [cfssl](https://github.com/cloudflare/cfssl), issue cert cho internal service. Hiểu khi nào nên dùng private CA.
- [ ] **8.19** Tìm hiểu [ECH (Encrypted Client Hello)](https://blog.cloudflare.com/announcing-encrypted-client-hello/) và xem project có enable qua Cloudflare được không.

### Ghi chú của tôi
```

```

---

## Module 9: CI/CD basics

### Cấp 1 - Cơ bản

- [ ] **9.1** Viết `deploy.sh` cho BE: cd vào thư mục app, `git pull`, `npm ci --production`, `pm2 reload ecosystem.config.js`. Chạy thử bằng tay trên server.
- [ ] **9.2** Viết `deploy.sh` cho FE: `git pull`, `npm ci`, `npm run build`, reload web server hoặc pm2. Chạy thử bằng tay.
- [ ] **9.3** Push project lên GitLab (hoặc dùng GitHub Actions nếu quen hơn). Tạo SSH key riêng cho CI/CD, add vào user `devops` trên server (`authorized_keys`).
- [ ] **9.4** Lưu private key vào CI/CD variables dạng MASKED + PROTECTED (GitLab) hoặc Secrets (GitHub).
- [ ] **9.5** Tạo `.gitlab-ci.yml` với 1 stage `deploy`: SSH vào server, chạy `deploy.sh`. Trigger bằng push lên `dev`.
- [ ] **9.6** Cấu hình: push lên `dev` → auto deploy. Push lên `main` → manual trigger (thêm `when: manual`).

### Cấp 2 - Vận dụng

- [ ] **9.7** Thêm stage `test`: chạy `npm test` trước `deploy`. Nếu test fail, không deploy.
- [ ] **9.8** Thêm stage `lint`: `npm run lint` fail → block merge (dùng GitLab push rules hoặc GitHub protected branches).
- [ ] **9.9** Build artifact ở CI (build FE thành `dist.tar.gz`), upload qua `scp`/`rsync` tới server. Server chỉ cần extract, không cần clone git + build (giảm load server prod).
- [ ] **9.10** Dùng cache CI (`cache: paths: - node_modules/`) để speed up build. Đo thời gian build trước/sau.
- [ ] **9.11** Khi CI fail, gửi notification Slack/Telegram/Discord qua webhook.
- [ ] **9.12** Dùng biến môi trường CI để phân biệt `CI_COMMIT_BRANCH == 'dev'` vs `'main'` và deploy vào folder khác nhau (staging vs production).
- [ ] **9.13** Log mọi bước trong `deploy.sh` bằng `set -euxo pipefail` để phát hiện lỗi sớm. KHÔNG echo biến chứa secret.

### Cấp 3 - Vận dụng cao

- [ ] **9.14** Zero-downtime deploy: dùng pm2 `reload` thay cho `restart` (cluster mode). Hoặc thiết kế blue-green với 2 port: chạy bản mới ở port B, đổi upstream Nginx, tắt bản cũ ở port A.
- [ ] **9.15** Health check sau deploy: sau khi restart, script phải `curl /health` ở loopback, expect 200. Nếu fail, auto rollback về version cũ.
- [ ] **9.16** Versioned releases: mỗi deploy lưu vào `releases/<timestamp>-<sha>/`, symlink `current → releases/xxx`. Giữ 5 release gần nhất. Rollback = đổi symlink.
- [ ] **9.17** Database migration trong pipeline: chạy `npm run migrate` trước deploy, handle lỗi migration sẽ không deploy code mới.
- [ ] **9.18** Tạo pipeline đa-env: `dev → staging → production`. Deploy staging auto, production manual với approval từ người khác (GitLab protected env).
- [ ] **9.19** Setup preview environment: mỗi MR/PR tạo ra 1 environment riêng ở subdomain `pr-123.yourdomain.com`. Teardown khi MR merge/close.
- [ ] **9.20** Thêm job security scan: [Trivy](https://github.com/aquasecurity/trivy) scan dependency + `npm audit`. Block deploy nếu có CVE HIGH/CRITICAL.
- [ ] **9.21** Secret management nâng cao: thay vì lưu trong CI vars, dùng [Vault](https://www.vaultproject.io/) hoặc AWS Secrets Manager, fetch khi runtime.
- [ ] **9.22** Migrate toàn bộ pipeline sang GitHub Actions (hoặc ngược lại). So sánh 2 platform, viết pros/cons.
- [ ] **9.23** Tự host 1 GitLab Runner trên server riêng (không dùng shared runner). Hiểu executor types: shell, docker, kubernetes.

### Ghi chú của tôi
```

```

---

## Capstone Challenges (Bài tập tổng hợp)

Sau khi xong toàn bộ 9 module, thử các thử thách tổng hợp này:

- [ ] **C1 - Rebuild from scratch**: Xoá sạch server, rebuild toàn bộ stack (hardening → DB → BE → FE → domain → reverse proxy → SSL → CI/CD) trong 4 giờ. Đo thời gian.
- [ ] **C2 - Multi-server**: Tách thành 3 server riêng (DB server, App server, Proxy server). Đảm bảo app vẫn chạy, hardening toàn bộ internal communication.
- [ ] **C3 - Disaster drill**: Giả lập disaster: xoá random 1 trong: DB data, BE app, Nginx config. Restore trong 30 phút từ backup/CI.
- [ ] **C4 - Pen-test**: Chạy [Nikto](https://github.com/sullo/nikto), [nmap](https://nmap.org/), [testssl.sh](https://github.com/drwetter/testssl.sh) audit toàn bộ stack. Fix mọi finding high/critical.
- [ ] **C5 - Load test**: Dùng [k6](https://k6.io/) hoặc [locust](https://locust.io/) ramp lên 1000 RPS. Ghi lại bottleneck và tune.
- [ ] **C6 - Document it all**: Viết runbook hoàn chỉnh `/docs/runbook.md` cho onboarding 1 DevOps mới. Họ phải làm theo runbook mà không hỏi bạn được gì.

---

## Lộ trình tiếp theo (Module 10-15 sắp tới)

- [ ] Module 10: Deploy using Docker
- [ ] Module 11: Deploy using Swarm
- [ ] Module 12: Logging
- [ ] Module 13: Deploy using Kubernetes
- [ ] Module 14: Advanced CI/CD
- [ ] Module 15: Monitoring

Khi tác giả cập nhật các module này, bổ sung exercises tương tự vào file này.
