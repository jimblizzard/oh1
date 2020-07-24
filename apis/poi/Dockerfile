#dockerfile to create image that runs unit tests 
# FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch
# RUN mkdir src
# COPY . /src/
# WORKDIR /src/tests/UnitTests
# RUN dotnet restore
# RUN dotnet test --logger trx; exit 0

#this setup is for running in an Azure DevOps pipeline
FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch
RUN echo $PWD
RUN ls
RUN mkdir src
COPY . ./src
WORKDIR /src
RUN ls
RUN dotnet restore 
RUN echo $PWD
RUN ls
WORKDIR /src/tests/UnitTests
RUN dotnet test --logger trx
#output test result to mounted volume 
#RUN dotnet test --logger trx --results-directory /var/temp