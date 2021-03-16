
VERSION ?= 1.0.0
ENV ?= Dev

# I use Singapore region becuase Lambda Containers isn't out in Sydney yet 
ECR_REGION ?= ap-southeast-2
ECR_ACCOUNT ?= 279158781928

ECR_URL = $(ECR_ACCOUNT).dkr.ecr.$(ECR_REGION).amazonaws.com
ECR_REPO = aws-sam-container-go
ECR_IMAGE = $(ECR_URL)/$(ECR_REPO):$(VERSION)


AWS_STACK_NAME=$(ENV)-SAMContainerGo

build: dockerAuth
	docker build -t $(ECR_IMAGE) hello-world/
	docker push $(ECR_IMAGE)

deploy:
	sam deploy --stack-name=$(AWS_STACK_NAME) \
		--capabilities CAPABILITY_IAM \
		--image-repository $(ECR_URL)/$(ECR_REPO)

invokeLocal:
	sam local invoke

runLocal:
	sam local start-api
	
# HELPERS
dockerAuth:
	aws ecr get-login-password --region $(ECR_REGION) | docker login --username AWS --password-stdin $(ECR_URL)

ecrCreate:
	aws ecr create-repository --repository-name $(ECR_REPO) --image-scanning-configuration scanOnPush=true