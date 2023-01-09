STACKNAME_BASE="my-sls-scraper-dev"
# This is a region that supports AWS Fargate
REGION="us-east-1"
PROFILE="sls-scraper"
IMAGE_NAME="my_sls_scraper"

build:
	docker build -t $(IMAGE_NAME):latest .

retag:
	docker tag $(IMAGE_NAME):latest \
		$(shell aws cloudformation --region $(REGION) --profile $(PROFILE) describe-stacks --stack-name $(STACKNAME_BASE) --query "Stacks[0].Outputs[?OutputKey=='ECRRepo'].OutputValue" --output text):latest
	@exec $(shell docker login --username AWS -p $(aws ecr get-login-password --region $(REGION) --profile $(PROFILE)) $(AWS_ACCOUNT_ID).dkr.ecr.$(REGION).amazonaws.com)
	docker push $(shell aws cloudformation --region $(REGION) --profile $(PROFILE) describe-stacks --stack-name $(STACKNAME_BASE) --query "Stacks[0].Outputs[?OutputKey=='ECRRepo'].OutputValue" --output text):latest