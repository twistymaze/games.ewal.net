FROM mcr.microsoft.com/dotnet/sdk:10.0.404@sha256:fe0fd4e93bd2fbb47c883b7a6369e5c8d3db8bf01cdc5981f6ee35fbdf3ff4fb AS build
WORKDIR /src
COPY ["GamesCacheUpdater.csproj", "./"]
RUN dotnet restore "GamesCacheUpdater.csproj"
COPY . .
RUN dotnet build "GamesCacheUpdater.csproj" -c Release -o /app/build
RUN dotnet publish "GamesCacheUpdater.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/runtime:10.0.15@sha256:80a51954ac70925e5783cb1ed35129a4243dcb1cddef20066b67346e17db284d
WORKDIR /app
COPY --from=build /app/publish .

# Only define non-sensitive environment variables with defaults
ENV UPDATE_INTERVAL_MINUTES="60"
ENV DO_SPACES_REGION="nyc3"

# Document required environment variables
# BGG_USERNAME - Your BGG username (required)
# BGG_PASSWORD - Your BGG password (optional)
# BGG_API_TOKEN - Your BGG API Bearer token (optional, but recommended for API authentication)
# DO_SPACES_KEY - Your Digital Ocean Spaces access key (required, keep secret!)
# DO_SPACES_SECRET - Your Digital Ocean Spaces secret key (required, keep secret!)
# DO_SPACES_BUCKET - Your Digital Ocean Spaces bucket name (required)
# DO_SPACES_REGION - Your Digital Ocean Spaces region (optional, defaults to nyc3)

ENTRYPOINT ["dotnet", "GamesCacheUpdater.dll"]
