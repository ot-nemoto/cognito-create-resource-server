# cognito-resource-server

## 概要

- CloudFormationでCognitoのResourceServerを作成するためのLambda関数

## 構成

![構成](https://github.com/ot-nemoto/cognito-create-resource-server/blob/images/cognito-create-resource-server.png)

## デプロイ

```sh
aws cloudformation create-stack \
    --stack-name cognito-resource-server \
    --capabilities CAPABILITY_IAM \
    --template-body file://template.yaml
```

## 使い方

FunctionのARNを取得

```sh
FUNCTION_ARN=$(aws cloudformation describe-stacks \
    --stack-name cognito-resource-server \
    --query 'Stacks[].Outputs[?OutputKey==`FunctionArn`].OutputValue' \
    --output text)
echo ${FUNCTION_ARN}
  #
```

Cognitoのユーザプールを作成

```sh
aws cloudformation create-stack \
    --stack-name example-cognito \
    --parameters ParameterKey=FunctionArn,ParameterValue=${FUNCTION_ARN} \
    --template-body file://cognito-template.yaml
```

## 参照

- [cfn-response Module](https://docs.aws.amazon.com/en_pv/AWSCloudFormation/latest/UserGuide/cfn-lambda-function-code-cfnresponsemodule.html)
