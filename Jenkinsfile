pipeline {
    agent any

    environment {
        JMETER_PATH = "/opt/jmeter/bin/jmeter"
        TEST_PLAN = "tests/mi_prueba.jmx"
        RESULT_DIR = "results"
        REPORT_DIR = "reportes"
    }

    stages {
        stage('Preparar entorno') {
            steps {
                echo "Limpieza previa..."
                sh "rm -rf ${RESULT_DIR} ${REPORT_DIR} || true"
                sh "mkdir -p ${RESULT_DIR} ${REPORT_DIR}"
            }
        }

        stage('Ejecutar prueba JMeter') {
            steps {
                echo "Ejecutando prueba con JMeter..."
                sh """
                    ${JMETER_PATH} -Xms128m -Xmx256m -n -t ${TEST_PLAN} -l ${RESULT_DIR}/resultados.jtl
                """
            }
        }

        stage('Generar reporte HTML') {
            steps {
                echo "Generando reporte HTML..."
                sh """
                    ${JMETER_PATH} -g ${RESULT_DIR}/resultados.jtl -o ${REPORT_DIR}
                """
            }
        }

        stage('Publicar artefactos') {
            steps {
                echo "Publicando artefactos..."
                archiveArtifacts artifacts: "${RESULT_DIR}/**/*.*", fingerprint: true
                archiveArtifacts artifacts: "${REPORT_DIR}/**/*.*", fingerprint: true
            }
        }
    }
}
