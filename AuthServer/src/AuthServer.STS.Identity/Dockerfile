FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/AuthServer.STS.Identity/AuthServer.STS.Identity.csproj", "src/AuthServer.STS.Identity/"]
COPY ["src/AuthServer.Admin.EntityFramework.Shared/AuthServer.Admin.EntityFramework.Shared.csproj", "src/AuthServer.Admin.EntityFramework.Shared/"]
COPY ["src/AuthServer.Shared/AuthServer.Shared.csproj", "src/AuthServer.Shared/"]
RUN dotnet restore "src/AuthServer.STS.Identity/AuthServer.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/AuthServer.STS.Identity"
RUN dotnet build "AuthServer.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AuthServer.STS.Identity.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "AuthServer.STS.Identity.dll"]