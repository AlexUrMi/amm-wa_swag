#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["wa_swag/wa_swag.csproj", "wa_swag/"]
RUN dotnet restore "wa_swag/wa_swag.csproj"
COPY . .
WORKDIR "/src/wa_swag"
RUN dotnet build "wa_swag.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "wa_swag.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "wa_swag.dll"]