# Deploy Vite React App lên GitHub Pages

Hướng dẫn chi tiết từng bước triển khai ứng dụng Vite + React lên GitHub Pages bằng cách sử dụng GitHub Actions với actions-gh-pages.

---

## Điều kiện tiên quyết

1. Một repository trên GitHub chứa dự án Vite + React của bạn.
2. Hiểu biết cơ bản về `npm` và Git.

---

## Hướng dẫn từng bước

### 1. Cập nhật `vite.config.js`

Thêm thuộc tính `base` vào file cấu hình Vite (`vite.config.js`). Thay `YOUR_REPOSITORY_NAME` bằng tên repository GitHub của bạn:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  base: '/YOUR_REPOSITORY_NAME/',
  plugins: [react()],
});
```

### 2. Cấu hình GitHub Actions

Tạo một file workflow GitHub Actions để tự động hóa quá trình triển khai. Lưu file sau dưới tên `.github/workflows/deploy.yml` trong repository của bạn:

```yaml
name: Deploy Vite React App to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Build the app
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: dist
          publish_branch: gh-pages
```

### 3. Push các thay đổi lên GitHub

Commit các thay đổi cho `vite.config.js`, và `.github/workflows/deploy.yml`. Sau đó, push lên nhánh `main`:

```bash
git add .
git commit -m "Cấu hình triển khai lên GitHub Pages"
git push origin main
```

### 4. Bật GitHub Pages

1. Truy cập vào repository trên GitHub.
2. Vào **Settings > Pages**.
3. Trong mục **Source**, chọn nhánh `gh-pages`.
4. Nhấn **Save**.

---

## Ghi chú

- Workflow sẽ tự động chạy mỗi khi bạn push thay đổi lên nhánh `main`.
- Ứng dụng sẽ được triển khai tại URL: `https://USERNAME.github.io/REPOSITORY_NAME/`.
