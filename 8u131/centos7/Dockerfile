FROM samloh84/centos:7

ARG VENDOR=oracle
ARG PRODUCT=jre
ARG PRODUCT_VERSION=8u131
ARG TEMP_DIR_ROOT=/tmp/${VENDOR}-${PRODUCT}
ARG TEMP_DIR=/${TEMP_DIR_ROOT}/${PRODUCT_VERSION}
ARG INSTALL_DIR_ROOT=/usr/java
ARG INSTALL_DIR=${INSTALL_DIR_ROOT}/jre1.8.0_131

ARG JRE_INSTALL_RPM_URL="http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jre-8u131-linux-x64.rpm"
ARG JRE_INSTALL_RPM=jre-${PRODUCT_VERSION}-linux-x64.rpm
ARG JRE_INSTALL_RPM_SHA256SUM=bd77dc59c6b8c64f9f7177bf1d5ca8d9f0b4a07707024b4f118a245165b7489d
ARG JCE_POLICY_ZIP_URL="http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip"
ARG JCE_POLICY_ZIP=jce_policy-8.zip
ARG JCE_POLICY_ZIP_SHA256SUM=f3020a3922efd6626c2fff45695d527f34a8020e938a49292561f18ad1320b59
ARG JCE_POLICY_ZIP_JAR_GLOB=UnlimitedJCEPolicyJDK8/*.jar

USER ${ROOT_UID}

RUN \
TEMP_DIR=/tmp/${VENDOR}-${PRODUCT}/${PRODUCT_VERSION} && \
yum-install unzip && \
mkdir -p ${TEMP_DIR} && \
pushd ${TEMP_DIR} && \
curl -LjSs -H "Cookie: oraclelicense=accept-securebackup-cookie" \
${JRE_INSTALL_RPM_URL} -o ${JRE_INSTALL_RPM} && \
curl -LjSs -H "Cookie: oraclelicense=accept-securebackup-cookie" \
${JCE_POLICY_ZIP_URL} -o ${JCE_POLICY_ZIP} && \
echo "${JRE_INSTALL_RPM_SHA256SUM}  ${JRE_INSTALL_RPM}" | sha256sum -c - && \
echo "${JCE_POLICY_ZIP_SHA256SUM}  ${JCE_POLICY_ZIP}" | sha256sum -c - && \
yum-install ${TEMP_DIR}/${JRE_INSTALL_RPM} && \
unzip -qq ${JCE_POLICY_ZIP} -d ${TEMP_DIR} && \
mv -f ${TEMP_DIR}/${JCE_POLICY_ZIP_JAR_GLOB} ${INSTALL_DIR}/lib/security && \
popd && \
rm -rf ${TEMP_DIR_ROOT}


ENV JAVA_HOME ${INSTALL_DIR}
ENV PATH ${JAVA_HOME}/bin:${PATH}

USER ${APP_UID}
