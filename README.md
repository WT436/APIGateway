"# APIGatewayEnvoy" 

Run Project 
di chuyển vào folder chưa file Dockerfile

docker build -t NAME_IMAGE .

docker run -d -p 8080:80 --name NAME_CONTAINER ID_OR_NAME_IMAGE

chạy lệnh compose : docker-compose up --build để chạy toàn bộ

Nếu sinh lỗi 
```
Win+R %userprofile% 
nếu chưa có : tạo mới .wslconfig
thêm cái quần què này vô :
[wsl2]
kernelCommandLine = vsyscall=emulate
khởi động lại wsl
wsl --shutdown
khởi động lại Docker Desktop

```

Cấu hình https : [Tại đây](https://docs.microsoft.com/en-us/aspnet/core/security/docker-https?view=aspnetcore-6.0)

Kiểm tra : 
``` 
Chạy lệnh : Win+R %userprofile% 
Vào thư mục : \.aspnet\https
không có file .pfx của dự án đang deploy
```

Fix : 
```
- mở PowerShell
- dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\{ name app here }.pfx -p { password here }
- cấu hình secrets cho certificate : dotnet user-secrets -p {đường dẫn đến file .csproj} set "Kestrel:Certificates:Development:Password" "crypticpassword"
```
fix view 
```
mở PowerShell
dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\API_One.pfx -p haiNam@99!!
Kết quả : A valid HTTPS certificate is already present.
Kiểm tra thư mục , có file API_One.pfx.
Tiếp tực chạy lệnh : dotnet dev-certs https --trust
Kiểm tra : C:\Users\{name}\AppData\Roaming\Microsoft\UserSecrets
và API_One.csproj> UserSecretsId có tồn tại trong thư mục trên (chắc chắn ko)
Chạy lệnh ở thư mục có file : API_One.csproj 
Thêm cái này vào : <UserSecretsId>Code GUID</UserSecretsId>
dotnet user-secrets set "Kestrel:Certificates:Development:Password" "haiNam@99!!"
Kết quả Successfully saved Kestrel:Certificates:Development:Password = haiNam@99!! to the secret store.
Kiểm tra lại lần nữa : 
Chạy thử :
docker build --pull -t aspnetapp .
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -v $env:APPDATA\microsoft\UserSecrets\:/root/.microsoft/usersecrets -v $env:USERPROFILE\.aspnet\https:/root/.aspnet/https/ aspnetapp

Kiểm tra : https://localhost:8001/WeatherForecast
```

Giới thiệu một chút về envoy proxy