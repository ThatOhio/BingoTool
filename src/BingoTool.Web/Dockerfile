FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/BingoTool.Web/BingoTool.Web.csproj", "src/BingoTool.Web/"]
RUN dotnet restore "src/BingoTool.Web/BingoTool.Web.csproj"
COPY . .
WORKDIR "/src/src/BingoTool.Web"
RUN dotnet build "./BingoTool.Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BingoTool.Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BingoTool.Web.dll"]
