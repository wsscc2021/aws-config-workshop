## AWS Config Workshop

AWS Config 를 설정하여 리소스 구성을 모니터링하고 규정 준수에 대한 감사 및 평가를 수행합니다.

나아가, 규정을 준수하지 못하는 리소스의 구성을 수정하는 작업을 자동화합니다.

Workshop을 통해 아래와 같은 내용을 확인할 수 있습니다.

- AWS 리소스 구성을 지속적으로 모니터링할 수 있습니다. 
  
  (리소스의 현재 구성을 확인할 수 있으며, 변경 전 이력(구성)도 확인할 수 있습니다.)

- Managed Config Rule를 통해 일반적인 규정 준수에 대한 감사 및 평가를 수행할 수 있습니다.

- Custom Config Rule를 직접 작성하고 배포하여 유연하게 규정 준수에 대한 감사 및 평가를 수행할 수 있습니다.

- Conformance Pack을 통해 여러 규칙 및 수정 작업을 단일 엔터티로 제공할 수 있습니다.

## Getting Started

1. Create cloudformation stack from `cfn-basic-resources.yaml`

    The template automatically provisioning below resources.

    - Three S3 Buckets (for config-snapshot || for compliant || for non-compliant)

    - SNS Topic for config-snapshot

    - Config Recorder

2. Create cloudformation stack from `cfn-managedRule-with-remediationAction.yaml`

    The template automatically provisioning below resources.

    - Config managed rule (`S3_BUCKET_SERVER_SIDE_ENCRYPTION_ENABLED`)
    
    - Remediation action (S3 Bucket default sse-encryption to AES256)

3. Create cloudformation stack from `cfn-customRule-with-lambda.yaml`

    The template automatically provisioning below resources.

    - Lambda function for evaluation config rule

    - Config custom rule using lambda

4. manually provisioning for custom rule using guard

    - Access to AWS Config console -> Rules tab -> Add rule

    - Create custom rule using guard

    - Name: `S3BucketPublicAccessBlockConfigurationCustomRule`

    - Rule content:
      ```
      rule public_access_block when
              resourceType == "AWS::S3::Bucket" {
          let config = supplementaryConfiguration.PublicAccessBlockConfiguration
          %config.blockPublicAcls == true
          %config.ignorePublicAcls == true
          %config.blockPublicPolicy == true
          %config.restrictPublicBuckets == true
      }
      ```
    
    - Scope of changes: `Resources`

    - Resources: `AWS resources` -> `AWS S3 Bucket`

5. manually provisioning for conformance pack

    - Access to AWS Config console -> Conformance packs tab -> Deploy conformance pack

    - Use sample template -> select `Operational Best Practices for Amazon S3`

    - Conformance pack name: `OperationalBestPracticesS3`