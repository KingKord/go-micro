SHELL=cmd.exe
FRONT_END_BINARY=frontApp.exe
BROKER_BINARY=brokerApp
AUTH_BINARY=authApp
LOGGER_BINARY=loggerServiceApp
MAIL_BINARY=mailApp
LISTENER_BINARY=listenerApp
## up: start all containers in the background without forcing build
up:
	@echo "Starting docker images..."
	docker-compose up -d
	@echo "Docker started!"

## docker_up_build: Build all projects and start docker compose
up_build: build_broker build_auth build_logger build_mail build_listener
	@echo "Stopping docker images (if running...)"
	docker-compose down
	@echo "Building (when required) and starting docker images..."
	docker-compose up --build -d
	@echo "Docker images built and started!"

build_broker:
	@echo Building broker binary...
	cd ..\broker-service && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build -o ${BROKER_BINARY} ./cmd/api
	@echo Done!

build_listener:
	@echo Building listener binary...
	cd ..\listener-service && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build -o ${LISTENER_BINARY} .
	@echo Done!

build_logger:
	@echo Building logger binary...
	cd ..\logger-service && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build -o ${LOGGER_BINARY} ./cmd/api
	@echo Done!


build_auth:
	@echo Building auth binary...
	cd ..\authentication-service && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build -o ${AUTH_BINARY} ./cmd/api
	@echo Done!

build_mail:
	@echo Building mail binary...
	cd ..\mail-service && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build -o ${MAIL_BINARY} ./cmd/api
	@echo Done!

build_front:
	@echo Building front end binary ...
	cd ..\front-end && set GOOS=windows&& set CGO_ENABLED=0 && go build -o ${FRONT_END_BINARY} ./cmd/web
	@echo Done

#
#build_front:
#	@echo "Building front end binary..."
#	cd ..\front_end && set CGO_ENABLED=0 && go build -o ${FRONT_END_BINARY} ./cmd/web
#	@echo "Done!"


start: build_front
	@echo "Starting front end"
	cd ../front-end && go build -o ${FRONT_END_BINARY} ./cmd/web
	cd ../front-end && ${FRONT_END_BINARY}

stop:
	@echo "Stopping back end..."
	@-pkill -SIGTERM -f "./frontApp"
	@echo "Stopped back end!"

## docker_down: Stop docker compose
down:
	@echo "Stopping docker images..."
	docker-compose down
	@echo "Docker stopped!"

test:
	@echo "Testing..."
	go test -v ./...


restart: down up_build