FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
ARG NUGET_AUTH_TOKEN
ARG NUGET_USER_NAME

WORKDIR /app
COPY . ./

COPY ./nuget.config.template ./nuget.config
RUN sed -i -e "s/USER/$NUGET_USER_NAME/g" -e "s/PW/$NUGET_AUTH_TOKEN/g" nuget.config 

RUN dotnet restore "/app/Axity.Users.Api/Axity.Users.Api.csproj"
RUN dotnet publish "/app/Axity.Users.Api/Axity.Users.Api.csproj" -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:6.0.15-alpine3.17
WORKDIR /app
COPY --from=build-env /app/out .

RUN apk add icu-libs --no-cache tzdata
RUN apk add libgdiplus --repository https://dl-3.alpinelinux.org/alpine/edge/testing/
RUN apk update

ENV TZ America/Mexico_City
EXPOSE 9091

ENTRYPOINT ["dotnet", "Axity.Users.Api.dll"]
