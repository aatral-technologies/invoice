#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.2-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 62467
EXPOSE 44344

FROM microsoft/dotnet:2.2-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["finapps.coreapi/finapps.coreapi.csproj", "finapps.coreapi/"]
RUN dotnet restore "finapps.coreapi/finapps.coreapi.csproj"
COPY . .
WORKDIR "/src/finapps.coreapi"
RUN dotnet build "finapps.coreapi.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "finapps.coreapi.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "finapps.coreapi.dll"]