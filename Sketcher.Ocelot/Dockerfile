#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-nanoserver-1903 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-nanoserver-1903 AS build
WORKDIR /src
COPY ["Sketcher.Ocelot/Sketcher.Ocelot.csproj", "Sketcher.Ocelot/"]
RUN dotnet restore "Sketcher.Ocelot/Sketcher.Ocelot.csproj"
COPY . .
WORKDIR "/src/Sketcher.Ocelot"
RUN dotnet build "Sketcher.Ocelot.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Sketcher.Ocelot.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Sketcher.Ocelot.dll"]