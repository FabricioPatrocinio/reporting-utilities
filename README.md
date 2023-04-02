# reporting-utilities
Report de email e sms usando um pouco de Event-Driven Architecture

#!/bin/bash
cd app
echo "================= DOWNLOADING DEPENDENCIES ================="
poetry install --no-dev
echo "====================== BUILD PACKAGES ======================"
zip -r9 ../app.zip . -x '*__pycache__/*' '*.venv*' '*.env*' '*poetry.lock*' '*.vscode/*' '*manage.py*' '*.md'
cd .venv/lib/*/site-packages
zip -ru9 ../../../../../app.zip . -x '*pip*' '*wheel*' '*setuptools*' '*pkg_resources*' '*_distutils_hack*' '*_virtualenv*' '*distutils-precedence*' '*__pycache__*'
cd ../../../../../
echo "========================== MD5SUM =========================="
md5sum app.zip
aws lambda update-function-code --function-name Backoffice_Queue_DEV_MaxLetsDelivery --zip-file fileb://app.zip --profile default
aws lambda update-function-code --function-name Backoffice_Queue_PROD_MaxLetsDelivery --zip-file fileb://app.zip --profile default
# aws lambda update-function-code --function-name Backoffice_Reports_DEV_MaxLetsDelivery --zip-file fileb://app.zip --profile default
# aws lambda update-function-code --function-name Backoffice_Reports_PROD_MaxLetsDelivery --zip-file fileb://app.zip --profile lets-prd
cd app
poetry install
