<!--[meta]
section: api
subSection: apps
title: GraphQL Schema Router
[meta]-->

# GraphQL Schema Router

> Lưu ý sau khi phiên bản KeystoneJS 5 dừng phát triển tính năng mới và chuyển
> sang chế độ duy trì để ra mắt phiên bản mới hơn. Chúng tôi đã dựa trên mã
> nguồn cũ này để phát triển một phiên bản khác với một số tính năng theo hướng
> microservices.

OcopJS - Điều hướng truy cập đến các Schema khác nhau. 🇻🇳

`SchemaRouterApp` cho phép bạn định nghĩa `routerFn` gồm có `(req, res)` và trả
về `routerId`, sử dụng để chọn giữa các GraphQL Schema khác nhau cùng tồn tại
trong `apiPath`.

## Sử dụng

```javascript
const { Ocop } = require('@ocopjs/ocop');
const { GraphQLAppPlayground } = require('@ocopjs/app-graphql-playground');
const { SchemaRouterApp } = require('@ocopjs/app-schema-router');
const { GraphQLApp } = require('@ocopjs/app-graphql');
const { AdminUIApp } = require('@ocopjs/app-admin-ui');

module.exports = {
  ocop: new Ocop(),
  apps: [
    new GraphQLAppPlayground({ apiPath })
    new SchemaRouterApp({
      apiPath,
      routerFn: (req) => req.session.ocopItemId ? 'private' : 'public',
      apps: {
        public: new GraphQLApp({ apiPath, schemaName: 'public', graphiqlPath: undefined }),
        private: new GraphQLApp({ apiPath, schemaName: 'private', graphiqlPath: undefined }),
      },
    }),
    new AdminUIApp()
  ],
};
```

## Cấu hình

| Cấu hình   | Loại       | Mặc định     | Mô tả                                                      |
| ---------- | ---------- | ------------ | ---------------------------------------------------------- |
| `apiPath`  | `String`   | `/admin/api` | Đường dẫn GraphQL API                                      |
| `routerFn` | `Function` | `() => {}`   | Hàm để dựa trên `(req, res)` để trả về `routerId`          |
| `apps`     | `Object`   | `{}`         | Thành phần chứa `routerId`như là một khoá của `GraphQLApp` |
