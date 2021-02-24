VERSION ?= latest

ECR_REGION ?= ap-southeast-2
ECR_ACCOUNT ?= 279158781928

ECR_URL = $(ECR_ACCOUNT).dkr.ecr.$(ECR_REGION).amazonaws.com
ECR_IMAGE = $(ECR_URL)/aws-lambda-container-nodejs:$(VERSION)

build: dockerAuth
	docker build -t $(ECR_IMAGE) .
	docker push $(ECR_IMAGE)

deploy:
	sls deploy

runLocal:
	docker run --rm -p 9000:8080 $(ECR_IMAGE)

invokeLocal:
	curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'

invokeRemote:
	sls invoke -f hello

# HELPERS
dockerAuth:
	aws ecr get-login-password --region $(ECR_REGION) | docker login --username AWS --password-stdin $(ECR_URL)

ecrCreate:
	aws ecr create-repository --repository-name aws-lambda-container-nodejs --image-scanning-configuration scanOnPush=true