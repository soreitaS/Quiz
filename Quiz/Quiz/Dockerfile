FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Quiz/Quiz/Quiz.csproj", "Quiz/Quiz/"]
COPY ["Quiz/Quiz.Client/Quiz.Client.csproj", "Quiz/Quiz.Client/"]
RUN dotnet restore "Quiz/Quiz/Quiz.csproj"
COPY . .
WORKDIR "/src/Quiz/Quiz"
RUN dotnet build "./Quiz.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Quiz.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Quiz.dll"]
