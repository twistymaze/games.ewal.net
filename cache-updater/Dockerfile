FROM mcr.microsoft.com/dotnet/sdk:10.0.402@sha256:0d99ebf1ccad15037b5c98c9fe8979d6b8dbac23812feae2277a0b277b5c5ffa AS build
WORKDIR /src
COPY ["GamesCacheUpdater.csproj", "./"]
RUN dotnet restore "GamesCacheUpdater.csproj"
COPY . .
RUN dotnet build "GamesCacheUpdater.csproj" -c Release -o /app/build
RUN dotnet publish "GamesCacheUpdater.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/runtime:10.0.13@sha256:939555726cae4c916d212323ac3c0792996dcac2614f00e82cc40fcf059e1af3
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
