# Verificar a compatibilidade do sistema operacional do host

FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Definindo a imagem SDK
FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Trocar para o usuário Administrator para evitar problemas de permissão
USER Administrator

# Copia o arquivo de projeto e faz o restore das dependências
COPY ["CRM.API.AUTH/CRM.API.AUTH.csproj", "CRM.API.AUTH/"]
RUN dotnet restore "CRM.API.AUTH/CRM.API.AUTH.csproj"

# Copia o restante do projeto e realiza o build
COPY . .
WORKDIR "/src/CRM.API.AUTH"
RUN dotnet build "CRM.API.AUTH.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Fase de publicação
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "CRM.API.AUTH.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Imagem final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CRM.API.AUTH.dll"]
