#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["UmbracoOnDocker/UmbracoOnDocker.csproj", "UmbracoOnDocker/"]
RUN dotnet restore "UmbracoOnDocker/UmbracoOnDocker.csproj"
COPY . .
WORKDIR "/src/UmbracoOnDocker"
RUN dotnet build "UmbracoOnDocker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "UmbracoOnDocker.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=build /src/UmbracoOnDocker/wwwroot/media ./wwwroot/media
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UmbracoOnDocker.dll"]