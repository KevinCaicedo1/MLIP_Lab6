pipeline {
    agent any

    stages {
        stage('Install Conda') {
            steps {
                sh '''
                echo '🔧 Installing Miniconda...'
                
                # Definir ruta de instalación
                CONDA_DIR="$HOME/miniconda3"
                export PATH="$CONDA_DIR/bin:$PATH"

                # Verificar si Conda ya está instalado
                if [ ! -d "$CONDA_DIR" ]; then
                    echo '🚀 Downloading and installing Miniconda...'
                    curl -fsSL https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh
                    bash miniconda.sh -b -p $CONDA_DIR
                    rm miniconda.sh
                    echo '✅ Miniconda installed successfully!'
                else
                    echo '✅ Miniconda is already installed.'
                fi

                # Inicializar Conda
                source "$CONDA_DIR/etc/profile.d/conda.sh"
                conda init bash
                '''
            }
        }

        stage('Create Virtual Env') {
            steps {
                sh '''
                echo '🌱 Creating and activating Conda environment...'
                
                # Ruta de Miniconda
                CONDA_DIR="$HOME/miniconda3"
                export PATH="$CONDA_DIR/bin:$PATH"
                source "$CONDA_DIR/etc/profile.d/conda.sh"

                # Crear entorno si no existe
                if ! conda env list | grep -q "test_env"; then
                    conda create -n test_env python=3.8 -y
                    echo '✅ Conda environment created!'
                else
                    echo '✅ Conda environment already exists.'
                fi
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo '📦 Installing pytest...'
                
                # Ruta de Miniconda
                CONDA_DIR="$HOME/miniconda3"
                export PATH="$CONDA_DIR/bin:$PATH"
                source "$CONDA_DIR/etc/profile.d/conda.sh"

                # Activar entorno y instalar pytest
                conda activate test_env
                conda install -n test_env pytest -y
                echo '✅ Dependencies installed.'
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo '🧪 Running pytest...'

                # Ruta de Miniconda
                CONDA_DIR="$HOME/miniconda3"
                export PATH="$CONDA_DIR/bin:$PATH"
                source "$CONDA_DIR/etc/profile.d/conda.sh"

                # Activar entorno y ejecutar pruebas
                conda run -n test_env --no-capture-output pytest

                echo '✅ pytest executed successfully.'
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying the project...'
            }
        }
    }
}
