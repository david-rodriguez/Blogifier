#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["src/Blogifier/Blogifier.csproj", "src/Blogifier/"]
COPY ["src/Blogifier.Core/Blogifier.Core.csproj", "src/Blogifier.Core/"]
COPY ["src/Blogifier.Shared/Blogifier.Shared.csproj", "src/Blogifier.Shared/"]
COPY ["src/Blogifier.Admin/Blogifier.Admin.csproj", "src/Blogifier.Admin/"]
RUN dotnet restore "src/Blogifier/Blogifier.csproj"
COPY . .
WORKDIR "/src/src/Blogifier"
RUN dotnet build "Blogifier.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Blogifier.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Blogifier.dll"]
