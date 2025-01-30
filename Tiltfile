allow_k8s_contexts('local')
docker_prune_settings(num_builds=1, keep_recent=1)

aissemble_version = '1.11.0-SNAPSHOT'

build_args = { 'DOCKER_BASELINE_REPO_ID': 'ghcr.io/',
               'VERSION_AISSEMBLE': aissemble_version}

# Kafka
yaml = helm(
    'test-test-547-deploy/src/main/resources/apps/kafka-cluster',
    values=['test-test-547-deploy/src/main/resources/apps/kafka-cluster/values.yaml',
        'test-test-547-deploy/src/main/resources/apps/kafka-cluster/values-dev.yaml']
)
k8s_yaml(yaml)

# Add deployment resources here

k8s_kind('SparkApplication', image_json_path='{.spec.image}')


yaml = local('helm template oci://ghcr.io/boozallen/aissemble-spark-application-chart --version %s --values test-test-547-pipelines/spark-pipeline/src/main/resources/apps/spark-pipeline-base-values.yaml,test-test-547-pipelines/spark-pipeline/src/main/resources/apps/spark-pipeline-dev-values.yaml' % aissemble_version)
k8s_yaml(yaml)
k8s_resource('spark-pipeline', port_forwards=[port_forward(4747, 4747, 'debug')], auto_init=False, trigger_mode=TRIGGER_MODE_MANUAL)

yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/spark-infrastructure',
   name='spark-infrastructure',
   values=['test-test-547-deploy/src/main/resources/apps/spark-infrastructure/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/spark-infrastructure/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/spark-operator',
   name='spark-operator',
   values=['test-test-547-deploy/src/main/resources/apps/spark-operator/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/spark-operator/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/policy-decision-point',
   name='policy-decision-point',
   values=['test-test-547-deploy/src/main/resources/apps/policy-decision-point/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/policy-decision-point/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/metadata',
   name='metadata',
   values=['test-test-547-deploy/src/main/resources/apps/metadata/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/metadata/values-dev.yaml']
)
k8s_yaml(yaml)

yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/pipeline-invocation-service',
   name='pipeline-invocation-service',
   values=['test-test-547-deploy/src/main/resources/apps/pipeline-invocation-service/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/pipeline-invocation-service/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-test-547-deploy/src/main/resources/apps/s3-local',
   name='s3-local',
   values=['test-test-547-deploy/src/main/resources/apps/s3-local/values.yaml',
       'test-test-547-deploy/src/main/resources/apps/s3-local/values-dev.yaml']
)
k8s_yaml(yaml)