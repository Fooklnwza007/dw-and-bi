# Instruction

1. เข้าไปใน Folder 06-analytics-engineering
```sh
cd 06-analytics-engineering
```
1. สร้าง web server postgres ด้วย docker port 3000
```sh
docker compose up
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/eb32d88c-0ae9-489b-9434-86b0007b64e8)


2. สร้าง ENV สำหรับการเขียน code Python และเก็บ package
```sh
python -m venv ENV
```
3. Activate เพื่อเข้าไปใน ENV เพื่อเก็บ package ต่างๆ
```sh
source ENV/bin/activate
```
4. download package สำหรับการใช้งาน dbt และ postgres
```sh
pip install dbt-core dbt-postgres
```
5. สร้างระบบฐานข้อมูลใน postgres และสร้าง profiles project ชื่อ ds525
```sh
dbt init
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/d77fbfde-50cc-42c1-b29b-65b01705c065)



6. เข้าไปใน Folder ds525 (ยังอยู่ใน ENV)
```sh
cd ds525
```
7. หลังจากสร้าง profiles project สร้าง file ใน folder ds525/models ด้วยชื่อ profiles.yml
และนำข้อมูลทั้งหมดจาก code ด้านล่าง มาใส่ไว้ใน file profiles.yml ที่เราสร้างใน ds525/models 
```sh
code /home/codespace/.dbt/profiles.yml
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/6e3f0dce-4948-43c8-8aca-7c860fe94eb0)


8. ทดสอบการ connection กับ postgres
```sh
dbt debug
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/22cf763d-f128-4096-ae11-fe01201b1003)


9. ทดสอบการ connection กับ postgres
```sh
dbt debug
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/b10c2f68-788d-4dd9-9ce0-6b94fb143a6e)


10. การนำข้อมูลขึ้นบน postgres จะใช้ข้อมูลจาก ds525/models .sql และใช้โครงสร้างในการ source ข้อมูลจาก _src.yml /
dbt_project.yml ใช้ในการกำหนดโครงสร้างชื่อหรือประเภท tables/view ที่สร้าง และไม่สนใจโครงสร้างของ folder
```sh
dbt run
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/4cf15b95-b85a-44c9-99e6-bd167705d520)


![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/25035386-4aeb-4406-b481-523208b47cb0)



11. จากนั้นรันคำสั่ง test เพื่อเช็ค data quality
```sh
dbt test
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/4cc47d15-bb45-4026-a4d8-5500007c9dc2)


12. ออกจาก ENV
```sh
deactivate
```
13. ปิดการทำงาน docker
```sh
docker compose down
```
