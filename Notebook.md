# MỤC LỤC
- [Mục 1: ESXi](#mục-1-esxi)
- [Mục 2: Ubuntu Linux](#mục-2-ubuntu-linux)
- [Mục 3: Docker](#mục-3-docker) 

## Mục 1: ESXi
### 1.1. Các lỗi ESXi:
1. Lỗi Kernel - do phần cứng bị thay đổi (Thay đổi Fan Speed, config trên ESXi,...)
    
    Vào Manage > Advanced Settings chỉnh VMKernel.Boot.vga thành True

2. Lỗi Failed - Invalid memory setting: memory reservation (sched.mem.min) should be equal to memsize(32768)

    Vào Edit của VM bị lỗi mở rộng phần Memory ra, set dòng Reservation dung lượng bằng dòng RAM

3. Lỗi Host bị icon cảnh báo màu vàng

    Vào Host > Actions chọn Exit maintainance mode

## Mục 2: Ubuntu Linux
### 2.1. Tăng Disk Ubuntu:
```
echo 1>/sys/class/block/sda/device/rescan
cfdisk => Resize => Write => Quit
pvresize /dev/sda3
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

### 2.2. Xóa log docker logs:
```
truncate -s 0 /var/lib/docker/containers/**/*-json.log
```

## MỤC 3: Docker
### 3.1. Cài đặt:
`curl -fsSL https://get.docker.com | sh`

### 3.2. Dockerfile Template:
#### 3.2.1. Dockerfile .NET:
```
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /app
# copy csproj and restore as distinct layers
COPY . .
RUN dotnet publish /app/Jhipster/ -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
EXPOSE 80
COPY --from=build /app/publish/* .
ENTRYPOINT ["dotnet", "jhipster.dll"]
```

### 