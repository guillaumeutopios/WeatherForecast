# microsoft a s?par? le sdk du runtime (compilation et ex?cution) 
# donc on devra utiliser 2 images diff?rentes (multi-stage)

# Image du build
FROM mcr.microsoft.com/dotnet/sdk:6.0-alpine AS build 
# AS build renome l'image (qui sera gardee en cache plus tard)
WORKDIR /app 
# on se d?place dans un dossier /app
COPY . . 
# on vient y copier les fichiers du projet
RUN dotnet restore 
# on restore les d?pendances
RUN dotnet publish -c Release -o bin 
# publish permet de publier le projet, ici dans le dossier bin 
# cela g?n?re les dll comme si on allait d?ployer le projet => port 80

# image du runtime
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app

COPY --from=build /app/bin . 
# on copie le dll du build vers le runtime pour pouvoir l'?x?cuter
CMD ["dotnet", "ApiWeatherForecast.dll"] 
# on ex?cute le dll

# commande ? utiliser pour build l'image :
# on devra se d?placer dans le dossier du dokerfile avant d'?x?cuter la commande suivante
# docker build -t apiweatherforecast-image .

# commande ? utiliser pour lancer le conteneur depuis l'image :
# docker run -d -p 4242:80 --name api apiweatherforecast-image