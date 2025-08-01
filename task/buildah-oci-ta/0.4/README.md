# buildah-oci-ta task

Buildah task builds source code into a container image and pushes the image into container registry using buildah tool.
In addition, it generates a SBOM file, injects the SBOM file into final container image and pushes the SBOM file as separate image using cosign tool.
When prefetch-dependencies task is activated it is using its artifacts to run build in hermetic environment.

## Parameters
|name|description|default value|required|
|---|---|---|---|
|ACTIVATION_KEY|Name of secret which contains subscription activation key|activation-key|false|
|ADDITIONAL_BASE_IMAGES|Additional base image references to include to the SBOM. Array of image_reference_with_digest strings|[]|false|
|ADDITIONAL_SECRET|Name of a secret which will be made available to the build with 'buildah build --secret' at /run/secrets/$ADDITIONAL_SECRET|does-not-exist|false|
|ADD_CAPABILITIES|Comma separated list of extra capabilities to add when running 'buildah build'|""|false|
|ANNOTATIONS|Additional key=value annotations that should be applied to the image|[]|false|
|ANNOTATIONS_FILE|Path to a file with additional key=value annotations that should be applied to the image|""|false|
|BUILDAH_FORMAT|The format for the resulting image's mediaType. Valid values are oci (default) or docker.|oci|false|
|BUILD_ARGS|Array of --build-arg values ("arg=value" strings)|[]|false|
|BUILD_ARGS_FILE|Path to a file with build arguments, see https://www.mankier.com/1/buildah-build#--build-arg-file|""|false|
|CACHI2_ARTIFACT|The Trusted Artifact URI pointing to the artifact with the prefetched dependencies.|""|false|
|COMMIT_SHA|The image is built from this commit.|""|false|
|CONTEXT|Path to the directory to use as context.|.|false|
|DOCKERFILE|Path to the Dockerfile to build.|./Dockerfile|false|
|ENTITLEMENT_SECRET|Name of secret which contains the entitlement certificates|etc-pki-entitlement|false|
|HERMETIC|Determines if build will be executed without network access.|false|false|
|IMAGE|Reference of the image buildah will produce.||true|
|IMAGE_EXPIRES_AFTER|Delete image tag after specified time. Empty means to keep the image tag. Time values could be something like 1h, 2d, 3w for hours, days, and weeks, respectively.|""|false|
|INHERIT_BASE_IMAGE_LABELS|Determines if the image inherits the base image labels.|true|false|
|LABELS|Additional key=value labels that should be applied to the image|[]|false|
|PREFETCH_INPUT|In case it is not empty, the prefetched content should be made available to the build.|""|false|
|PRIVILEGED_NESTED|Whether to enable privileged mode, should be used only with remote VMs|false|false|
|SBOM_TYPE|Select the SBOM format to generate. Valid values: spdx, cyclonedx. Note: the SBOM from the prefetch task - if there is one - must be in the same format.|spdx|false|
|SKIP_SBOM_GENERATION|Skip SBOM-related operations. This will likely cause EC policies to fail if enabled|false|false|
|SKIP_UNUSED_STAGES|Whether to skip stages in Containerfile that seem unused by subsequent stages|true|false|
|SOURCE_ARTIFACT|The Trusted Artifact URI pointing to the artifact with the application source code.||true|
|SQUASH|Squash all new and previous layers added as a part of this build, as per --squash|false|false|
|STORAGE_DRIVER|Storage driver to configure for buildah|overlay|false|
|TARGET_STAGE|Target stage in Dockerfile to build. If not specified, the Dockerfile is processed entirely to (and including) its last stage.|""|false|
|TLSVERIFY|Verify the TLS on the registry endpoint (for push/pull to a non-TLS registry)|true|false|
|WORKINGDIR_MOUNT|Mount the current working directory into the build using --volume $PWD:/$WORKINGDIR_MOUNT. Note that the $PWD will be the context directory for the build (see the CONTEXT param).|""|false|
|YUM_REPOS_D_FETCHED|Path in source workspace where dynamically-fetched repos are present|fetched.repos.d|false|
|YUM_REPOS_D_SRC|Path in the git repository in which yum repository files are stored|repos.d|false|
|YUM_REPOS_D_TARGET|Target path on the container in which yum repository files should be made available|/etc/yum.repos.d|false|
|caTrustConfigMapKey|The name of the key in the ConfigMap that contains the CA bundle data.|ca-bundle.crt|false|
|caTrustConfigMapName|The name of the ConfigMap to read CA bundle data from.|trusted-ca|false|

## Results
|name|description|
|---|---|
|IMAGE_DIGEST|Digest of the image just built|
|IMAGE_REF|Image reference of the built image|
|IMAGE_URL|Image repository and tag where the built image was pushed|
|SBOM_BLOB_URL|Reference of SBOM blob digest to enable digest-based verification from provenance|

