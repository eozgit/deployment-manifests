# --- STAGE 1: Build the .NET application ---
FROM mcr.microsoft.com/dotnet/sdk:9.0-preview AS build
WORKDIR /src

# Declare arguments for the repository URL and branch
ARG REPO_URL
ARG BRANCH=main

# 1. Install Git
RUN apt-get update && apt-get install -y git

# 2. Clone the repository into the working directory
RUN git clone --depth 1 --branch ${BRANCH} ${REPO_URL} .

# Restore dependencies
RUN dotnet restore api/api.csproj

# Publish the application for release
RUN dotnet publish -c Release -o /app/publish --no-restore api/api.csproj

# --- STAGE 2: Create the final runtime image ---
# Use the smaller ASP.NET runtime image
FROM mcr.microsoft.com/dotnet/aspnet:9.0-preview AS final
WORKDIR /app
EXPOSE 8080

# Copy the published output from the build stage
COPY --from=build /app/publish .

# Set the entry point to run the published application
ENTRYPOINT ["dotnet", "api.dll"]
