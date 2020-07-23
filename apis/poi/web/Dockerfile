FROM microsoft/dotnet:2.1-sdk AS build-env
WORKDIR /app

# Modified based on ColinsAlmCorner blog post: 
#   https://colinsalmcorner.com/net-core-multi-stage-dockerfile-with-test-and-code-coverage-in-azure-pipelines/

# copy csproj and restore as distinct layers
COPY *.csproj ./

# copy the unit tests
COPY ../UnitTests ./UnitTests

# restore for everything
RUN dotnet restore

# test 
# use the label to identify this layer later
LABEL test="true"

# install the report generator tool
RUN dotnet tool install dotnet-reportgenerator-globaltool --version 4.0.6 --tool-path /tools 
# run the test
RUN dotnet test --results-directory /testresults --logger "trx;LogFileName=test_results.xml" ./UnitTests/UnitTests.csproj

# copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out
COPY ./appsettings.*.json /app/out/
COPY ./appsettings.json /app/out/

# build runtime image
FROM microsoft/dotnet:2.1-aspnetcore-runtime	
WORKDIR /app

ENV SQL_USER="demousersa" \
SQL_PASSWORD="demo@pass123" \
SQL_SERVER="openhackkbi70sk9sql.database.windows.net" \
SQL_DBNAME="mydrivingDB" \
WEB_PORT="8080" \
WEB_SERVER_BASE_URI="http://0.0.0.0" \
ASPNETCORE_ENVIRONMENT="Production"

COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "poi.dll"]

