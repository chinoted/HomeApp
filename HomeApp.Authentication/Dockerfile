#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 62849
EXPOSE 44333

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["HomeApp.Authentication/HomeApp.Authentication.csproj", "HomeApp.Authentication/"]
RUN dotnet restore "HomeApp.Authentication/HomeApp.Authentication.csproj"
COPY . .
WORKDIR "/src/HomeApp.Authentication"
RUN dotnet build "HomeApp.Authentication.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "HomeApp.Authentication.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "HomeApp.Authentication.dll"]