# Dockerize .Net Core App

### Create a .Net Core App from Visual Studio Code

Create a new folder in file system. Open the folder in [Visual Studio Code](https://code.visualstudio.com/). Launch the command palette by ` ctrl + ~ `

#### Steps:
* On the command terminal type ``` dotnet new mvc ```. This step will create .Net Core MVC project in the current folder and restores the nuget packages.
* You can try running this code by ``` dotnet run ```. This step starts the application. You can browse exposed URL in browser and see your app running.
### Add this code in github. 
* ``` git remote add origin https://github.com/<username>/<reponame>.git ```
* ``` git add . ```
* ``` git commit -m "Initial commit" ```
* ``` git push -u origin master ```

### Multi-arch based .Net core docker images
* Docker has a [multi-arch](https://github.com/moby/moby/issues/15866) feature that [microsoft/dotnet-nightly](https://hub.docker.com/r/microsoft/dotnet-nightly/) recently started utilizing. The plan is to port this to the [official microsoft/dotnet repo](https://hub.docker.com/r/microsoft/dotnet/) shortly. The multi-arch feature allows a single tag to be used across multiple machine configurations. Without this feature each architecture/OS/platform requires a unique tag. 
* Create a new file in the folder. Rename it as ``` Dockerfile ```
* ``` dockerfile
      FROM microsoft/aspnetcore-build-nightly AS builder
      WORKDIR /source
      COPY *.csproj .
      RUN dotnet restore
      COPY . .
      RUN dotnet publish --output /app/ --configuration Release
      FROM microsoft/aspnetcore-nightly
      WORKDIR /app
      COPY --from=builder /app .
      ENTRYPOINT ["dotnet", "multiarchapp.dll"]
  ```

