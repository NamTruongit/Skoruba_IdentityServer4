FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/AuthServer.Admin.Api/AuthServer.Admin.Api.csproj", "src/AuthServer.Admin.Api/"]
COPY ["src/AuthServer.Admin.EntityFramework.Shared/AuthServer.Admin.EntityFramework.Shared.csproj", "src/AuthServer.Admin.EntityFramework.Shared/"]
COPY ["src/AuthServer.Admin.EntityFramework.SqlServer/AuthServer.Admin.EntityFramework.SqlServer.csproj", "src/AuthServer.Admin.EntityFramework.SqlServer/"]
COPY ["src/AuthServer.Shared/AuthServer.Shared.csproj", "src/AuthServer.Shared/"]
COPY ["src/AuthServer.Admin.EntityFramework.PostgreSQL/AuthServer.Admin.EntityFramework.PostgreSQL.csproj", "src/AuthServer.Admin.EntityFramework.PostgreSQL/"]
COPY ["src/AuthServer.Admin.EntityFramework.MySql/AuthServer.Admin.EntityFramework.MySql.csproj", "src/AuthServer.Admin.EntityFramework.MySql/"]
RUN dotnet restore "src/AuthServer.Admin.Api/AuthServer.Admin.Api.csproj"
COPY . .
WORKDIR "/src/src/AuthServer.Admin.Api"
RUN dotnet build "AuthServer.Admin.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AuthServer.Admin.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "AuthServer.Admin.Api.dll"]