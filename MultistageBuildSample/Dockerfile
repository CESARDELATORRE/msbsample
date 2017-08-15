# base image for F5 debug
FROM microsoft/aspnetcore:2.0 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS develop
ARG BuildConfiguration
WORKDIR /src
COPY *.sln ./
COPY MultistageBuildSample/MultistageBuildSample.csproj MultistageBuildSample/
COPY ClassLibrary1/ClassLibrary1.csproj ClassLibrary1/
RUN dotnet restore
COPY . .
WORKDIR /src/MultistageBuildSample
RUN dotnet build -c ${BuildConfiguration:-Debug}
ENTRYPOINT ["dotnet", "MultistageBuildSample.dll"]

FROM develop AS publish
ARG BuildConfiguration
WORKDIR /src/MultistageBuildSample
RUN dotnet publish -c ${BuildConfiguration:-Release} -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "MultistageBuildSample.dll"]
