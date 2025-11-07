### 1. ckanext-geoview:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://github.com/ckan/ckanext-geoview.git@v0.2.2#egg=ckanext-geoview'
```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม geo_view geojson_view shp_view ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... geo_view geojson_view shp_view
    - ckan.views.default_views (เติม geo_view geojson_view shp_view ต่อจากที่มีอยู่แล้ว)
        > ckan.views.default_views = ... geo_view geojson_view shp_view
```
```sh
sudo supervisorctl reload
```

### 2. ckanext-xloader:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://gitlab.nectec.or.th/opend/ckanext-xloader.git#egg=ckanext-xloader'

pip install -r src/ckanext-xloader/requirements.txt

pip install -U requests[security]
```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - เพิ่ม config ถัดจากบรรทัด [app:main]
        > ckanext.xloader.just_load_with_messytables = true
        > ckanext.xloader.ssl_verify = false
        > ckan.datatables.state_saving = false
    - ckan.plugins (เติม xloader datastore ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... xloader datastore
    - ckanext.xloader.jobs_db.uri (เพิ่ม config นี้ ถัดจาก sqlalchemy.url และให้มีค่าเหมือนกัน)
        > ckanext.xloader.jobs_db.uri = postgresql://ckan_default:{password1}@localhost/ckan_default
```
```sh
sudo supervisorctl reload
```

หากต้องการกำหนดให้ xloader submit all อัตโนมัติเข้า DataStore ทุกวัน ให้ set cronjob ดังนี้
```sh
crontab -e
```
เพิ่มคำสั่งต่อไปนี้

    @daily /usr/lib/ckan/default/bin/paster --plugin=ckanext-xloader xloader submit all -c /etc/ckan/default/ckan.ini


### 3. ckanext-pdfview:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://github.com/ckan/ckanext-pdfview.git@0.0.8#egg=ckanext-pdfview'
```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม pdf_view ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... pdf_view
    - ckan.views.default_views (เติม pdf_view ต่อจากที่มีอยู่แล้ว)
        > ckan.views.default_views = ... pdf_view
```
```sh
sudo supervisorctl reload
```

### 4. ckanext-dcat:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://github.com/ckan/ckanext-dcat.git@v2.3.0#egg=ckanext-dcat'

pip install -r src/ckanext-dcat/requirements.txt
```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม dcat dcat_json_interface structured_data ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... dcat dcat_json_interface structured_data
```
```sh
sudo supervisorctl reload
```

### 5. ckanext-scheming:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://github.com/ckan/ckanext-scheming.git@release-3.1.0#egg=ckanext-scheming'

```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม scheming_datasets ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... scheming_datasets
```
```sh
sudo supervisorctl reload
```

### 6. ckanext-hierarchy:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://github.com/ckan/ckanext-hierarchy.git@v1.2.2#egg=ckanext-hierarchy'

```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม hierarchy_display hierarchy_form ต่อจากที่มีอยู่แล้ว)
        > ckan.plugins = ... hierarchy_display hierarchy_form
```
```sh
sudo supervisorctl reload
```

### 7. ckanext-opendstats:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://gitlab.nectec.or.th/opend/ckanext-opendstats.git@dev-py3#egg=ckanext-opendstats'
```
แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม opendstats ต่อจาก stats)
        > ckan.plugins = ... stats opendstats ...
```
```sh
sudo supervisorctl reload
```

รันคำสั่ง db-init เพื่อสร้างตารางข้อมูลสำหรับ ckanext-opendstats
```
sudo /usr/lib/ckan/default/bin/ckan -c /etc/ckan/default/ckan.ini opendstats db-init

#*** ขั้นตอนนี้อาจจะใช้เวลานานขึ้นอยู่กับจำนวนข้อมูล tracking และ ชุดข้อมูลของแต่ละหน่วยงานอาจจะต้องรันด้วย tmux
```

เพิ่ม crontab เพื่อดึงข้อมูลใหม่ในทุกวัน
```
    crontab -e
```
```
    @daily /usr/lib/ckan/default/bin/ckan -c /etc/ckan/default/ckan.ini opendstats fetch
```

### 8. ckanext-showcase:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://gitlab.nectec.or.th/opend/dev-python3/ckanext-showcase.git#egg=ckanext-showcase'

```

แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม showcase ไว้หน้าสุด)
        > ckan.plugins = showcase ...
```
```sh
sudo supervisorctl reload
```

### 9. ckanext-thai_gdc:
```sh
source /usr/lib/ckan/default/bin/activate

cd /usr/lib/ckan/default

pip install -e 'git+https://gitlab.nectec.or.th/opend/ckanext-thai_gdc.git#egg=ckanext-thai_gdc'

pip install -r src/ckanext-thai-gdc/requirements.txt
```

แก้ไขไฟล์ config ของ CKAN ดังนี้:
```sh
sudo vi /etc/ckan/default/ckan.ini
```
```sh
    - ckan.plugins (เติม thai_gdc ไว้หน้าสุด รวมถึงให้อยู่ก่อน showcase stats opendstats datatables_view scheming_datasets hierarchy_display hierarchy_form ด้วย)
        > ckan.plugins = thai_gdc showcase stats opendstats ... datatables_view ... scheming_datasets hierarchy_display hierarchy_form
```
```sh
sudo supervisorctl reload
```
