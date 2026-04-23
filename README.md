# ProjectSem3

## Mô tả dự án

ProjectSem3 là dự án Online Book Store (nhà sách trực tuyến) phục vụ đồ án học kỳ, được tổ chức theo mô hình monorepo gồm backend và 2 frontend.

Hệ thống tập trung vào các chức năng chính:
- Xác thực người dùng bằng JWT và refresh token.
- Quản lý dữ liệu nhà sách như người dùng, sách/sản phẩm, danh mục, đơn hàng, thanh toán, khuyến mãi.
- Cung cấp giao diện cho người dùng cuối và khu vực quản trị/legacy.

Monorepo cho dự án học kỳ gồm:
- Backend API: ASP.NET Core 8 + Entity Framework Core + JWT
- Frontend quản trị/legacy: React + TypeScript + Vite (`FrontEnd`)
- Frontend người dùng: React + TypeScript + Vite (`frontend-users`)

## Cấu trúc dự án

- `baseBackend`: Web API, kết nối SQL Server, xác thực JWT + refresh token
- `FrontEnd`: giao diện admin/user (router riêng)
- `frontend-users`: giao diện user mới (trang chủ, login, sản phẩm, cửa hàng...)

## Công nghệ chính

- Backend: `.NET 8`, `ASP.NET Core Web API`, `EF Core`, `SQL Server`, `Swagger`
- Auth: `JWT Bearer`, `BCrypt` hash password, `refresh token`
- Frontend: `React 18`, `TypeScript`, `Vite`, `Redux Toolkit`, `Axios`, `Sass`

## Yêu cầu môi trường

- .NET SDK 8.x
- Node.js 18+ (khuyến nghị 20+)
- SQL Server (hoặc SQL Server Express)
- npm

## Cấu hình nhanh

### 1) Backend (`baseBackend`)

File cấu hình chính:
- `baseBackend/appsettings.json`
- `baseBackend/Properties/launchSettings.json`

Mặc định backend chạy tại: `http://localhost:5047` (có Swagger tại `/swagger`).

Bạn cần cập nhật:
- `ConnectionStrings:ConnectDb` theo máy của bạn
- `Jwt:Key`, `Jwt:Issuer`, `Jwt:Audience`, `Jwt:PublicKey`

> Lưu ý bảo mật: không nên commit password DB/JWT secret thật vào git. Nên dùng Secret Manager hoặc biến môi trường.

### 2) Frontend admin/legacy (`FrontEnd`)

File env:
- `FrontEnd/.env`

Biến đang dùng:
- `VITE_REACT_APP_BACK_END_LINK="http://localhost:5047/api/"`

### 3) Frontend user (`frontend-users`)

File env:
- `frontend-users/.env`

Biến đang dùng:
- `VITE_API_BACKEND='http://localhost:5047/api/'`

## Cách chạy dự án

Mở 3 terminal riêng.

### Chạy backend

```bash
cd baseBackend
dotnet restore
dotnet run
```

Nếu cần cập nhật database bằng migration:

```bash
cd baseBackend
dotnet ef database update
```

### Chạy frontend `FrontEnd`

```bash
cd FrontEnd
npm install
npm run dev
```

Mac dinh Vite chay o `http://localhost:5173`.

Mặc định Vite chạy ở `http://localhost:5173`.

### Chạy frontend `frontend-users`

```bash
cd frontend-users
npm install
npm run dev
```

Nếu chạy đồng thời 2 frontend, hãy set cổng khác nhau để tránh trùng port (ví dụ một bên `5173`, một bên `5174`).

## CORS hiện tại

Backend đang allow:
- `http://localhost:5173`
- `http://localhost:3000`

Nếu frontend chạy cổng khác, cần sửa trong `baseBackend/Program.cs`.

## API chính hiện có

Base URL: `http://localhost:5047/api`

### Auth
- `POST /Auth/Login`: đăng nhập, trả `token` (và `refreshToken` nếu nhớ đăng nhập)
- `POST /Auth/RefreshToken`: cấp lại access token bằng refresh token
- `GET /Auth` (Authorize): kiểm tra token

### User
- `GET /User`: lấy danh sách user
- `POST /User`: tạo user mới (password được hash BCrypt)

Swagger: `http://localhost:5047/swagger`

## Route frontend (tham khảo nhanh)

### `frontend-users`
- `/home`
- `/login`
- `/store`
- `/all-products`

### `FrontEnd`
- `/` (user home)
- `/login`
- `/admin`
- `/admin/about`

## Lệnh hữu ích

### `FrontEnd`
- `npm run dev`
- `npm run build`
- `npm run lint`

### `frontend-users`
- `npm run dev`
- `npm run build`
- `npm run lint`
- `npm run lint:fix`
- `npm run prettier`
- `npm run prettier:fix`

## Ghi chú

- Một số controller trong backend đang để ở trạng thái comment/development (`ResourceController`, `ActionController`).
- Dự án đang có 2 frontend song song; nên thống nhất hướng sử dụng để tránh trùng lặp chức năng.

## Ảnh mô tả hệ thống

### Quản trị đơn hàng
![Danh sách đơn hàng](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-f57542e3-5913-41ea-9442-c57367fc0152.png)
![Chi tiết đơn hàng](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-16eebf7f-d0d5-478b-9637-bcba948c1919.png)
![Cập nhật trạng thái đơn](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-fb6c1cad-2d36-494a-b2ae-a6a9cefbf143.png)

### Quản lý danh mục
![Màn hình danh mục sách](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-14a44bf1-8dd0-4e40-a2ed-c549a7ee405b.png)

### Giao diện người dùng
![Trang chủ nhà sách](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-ac792d4f-58ee-4554-a71b-4099426b8f18.png)
![Trang thanh toán](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-fb47ac09-2cfe-4697-b663-46d5b7d7a918.png)

### Dashboard quản trị
![Trang tổng quan quản trị](C:/Users/nguye/.cursor/projects/j-nh-p-ProjectSem3/assets/c__Users_nguye_AppData_Roaming_Cursor_User_workspaceStorage_9710ae4f539fb2de48dd66cd36984b24_images_image-c20cbd74-5f68-4f21-8e80-8c58723db13f.png)