FROM mcr.microsoft.com/dotnet/sdk:11.0.100@sha256:65f3c7c6a25a239ab3e6719f2896147e7b68242ff2d8818dd70498bde6ef8bea AS build
WORKDIR /src
COPY ["GamesCacheUpdater.csproj", "./"]
RUN dotnet restore "GamesCacheUpdater.csproj"
COPY . .
RUN dotnet build "GamesCacheUpdater.csproj" -c Release -o /app/build
RUN dotnet publish "GamesCacheUpdater.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/runtime:11.0.0@sha256:3fe5f5495141c97986a49d3a4e76061f6225c3f93f94e8a7e522781376193863
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
