FROM microsoft/dotnet:2.1-sdk AS build-env
WORKDIR /app
COPY . ./
RUN ls
# copy csproj and restore as distinct layers
RUN dotnet restore tests/UnitTests/UnitTests.csproj

# copy everything else and build

#Unit Test
RUN ls
RUN dotnet test tests/UnitTests/UnitTests.csproj
RUN dotnet publish web/ -c Release -o out
COPY ./web/appsettings.*.json /app/out/
COPY ./web/appsettings.json /app/out/

# build runtime image
FROM microsoft/dotnet:2.1-aspnetcore-runtime
WORKDIR /app

ENV SQL_USER="YourUserName" \
SQL_PASSWORD="changeme" \
SQL_SERVER="changeme.database.windows.net" \
SQL_DBNAME="mydrivingDB" \
WEB_PORT="8080" \
WEB_SERVER_BASE_URI="http://0.0.0.0" \
ASPNETCORE_ENVIRONMENT="Production"

COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "poi.dll"]
