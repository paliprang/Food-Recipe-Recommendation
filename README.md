# Food Recipe Recommendation System (ระบบแนะนำเมนูอาหาร)

โครงงานนี้มีวัตถุประสงค์เพื่อพัฒนาระบบแนะนำสูตรอาหาร เพื่อแก้ไขปัญหาการค้นหาและการตัดสินใจเลือกเมนูอาหารของผู้ใช้งาน รวมถึงปัญหาการได้รับคำแนะนำที่ไม่ตรงกับความชอบ โดยนำเทคนิค Recommendation System มาใช้วิเคราะห์และจัดอันดับสูตรอาหารที่มีแนวโน้มตรงกับความสนใจของผู้ใช้งาน โดยใช้ชุดข้อมูล **Food.com - Recipes and Reviews** ซึ่งประกอบด้วยข้อมูลสูตรอาหารและข้อมูลการให้คะแนนและรีวิวจากผู้ใช้งาน โดยมีการใช้ 3 โมเดลหลักดังนี้:

1. **Popularity-Based Recommendation:** การแนะนำสูตรอาหารโดยพิจารณาจากความนิยมของสูตรอาหาร
2. **Content-Based Filtering:** การแนะนำสูตรอาหารโดยพิจารณาจากคุณลักษณะของสูตรอาหารที่ผู้ใช้สนใจ
3. **User-Based Collaborative Filtering:** การแนะนำสูตรอาหารโดยอาศัยข้อมูลการให้คะแนนของผู้ใช้ที่มีความสนใจคล้ายคลึงกัน]

## โครงสร้างไฟล์ (Project Structure)
- G009_FoodRecipe_RecommendationSystem.ipynb : ไฟล์ Notebook หลักสำหรับการเตรียมข้อมูล เทรนโมเดล และประเมินผลการทดลอง
- requirements.txt : รายการ Library และเลขเวอร์ชันทั้งหมดที่ใช้ในโปรเจกต์
- recipes.csv และ reviews.csv : ชุดข้อมูลหลักที่ใช้ในการทดลอง

## การติดตั้ง (Dependencies)
โปรเจกต์นี้พัฒนาด้วยภาษา Python สามารถติดตั้ง Library ที่จำเป็นทั้งหมดได้ผ่านคำสั่ง:

    pip install -r requirements.txt

## ข้อมูลและการตั้งค่าเพื่อการทำซ้ำ (Dataset & Reproducibility)
- Dataset: ใช้ไฟล์ recipes.csv เป็นข้อมูลเกี่ยวกับสูตรอาหาร และ reviews.csv เป็นข้อมูลเกี่ยวการรีวิวและการให้คะแนนสูตรอาหารจากผู้ใช้งาน
  ซึ่งแนบไว้ใน Repository นี้แล้ว
- การควบคุมค่าสุ่ม (Random Seed): มีการกำหนดค่า random_state=42 ในขั้นตอนการสุ่มเลือกผู้ใช้ 1,000 คน  เพื่อให้การรันซ้ำในทุกครั้งได้ผลลัพธ์ตรงกัน

## วิธีการรันโค้ด (How to Run)
1. ดาวน์โหลดไฟล์ G009_FoodRecipe_RecommendationSystem.ipynb จาก Repository นี้
2. ดาวน์โหลดชุดข้อมูลจาก Kaggle (https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews) และแตกไฟล์ `.zip` เพื่อนำไฟล์ `recipes.csv` และ `reviews.csv` ออกมา
3. เปิดเว็บไซต์ **Google Colab** ([https://colab.research.google.com/](https://colab.research.google.com/)) และอัปโหลดไฟล์ G009_FoodRecipe_RecommendationSystem.ipynb เข้าสู่ระบบ
4. คลิกที่ไอคอนรูปโฟลเดอร์ (Files) บนแถบเมนูด้านซ้ายมือ แล้วอัปโหลดไฟล์ recipes.csv และ reviews.csv ลงไปในหน้าหลัก
5. กดที่เมนูด้านบนเลือก **`Runtime` > `Run all`** เพื่อรันโค้ดทั้งหมดตั้งแต่ต้นจนจบ


## ผลลัพธ์การทดลอง (Experimental Results)
จากการประเมินประสิทธิภาพบนกลุ่มผู้ใช้ทดสอบจำนวน 1,000 คน (แบ่งข้อมูลแบบ Time-based Split สัดส่วน 80:20 และประเมินจากเมนูที่มี Rating ≥ 4 ใน Test Set) ได้ผลลัพธ์เปรียบเทียบดังนี้:

| โมเดล (Model) | Recall@10 (Primary) | Precision@10 | Recall@5 | Precision@5 |
| :--- | :---: | :---: | :---: | :---: |
| **M0: Popularity-Based (Baseline)** | **0.001733** | **0.000800** | **0.001233** | **0.001400** |
| **M1: Content-Based Filtering** | 0.000333 | 0.000100 | 0.000000 | 0.000000 |
| **M1+: Content-Based (เพิ่มฟีเจอร์ Ingredients)** | 0.000400 | 0.000200 | 0.000000 | 0.000000 |
| **M2: User-Based Collaborative Filtering** | 0.001524 | 0.000600 | 0.001000 | 0.000800 |

- **สรุปผลการเปรียบเทียบโมเดล:** 
  - **Popularity-Based Baseline (M0)** ให้ค่าประสิทธิภาพเชิงปริมาณสูงที่สุดในทุกตัวชี้วัด (Recall@10 = `0.001733` และ Precision@5 = `0.001400`) และสามารถสร้างรายการแนะนำได้ครอบคลุมทุกกลุ่มผู้ใช้แม้ในสภาพข้อมูลที่มีความเบาบางสูง (Sparsity 99.99%)
  - **User-Based Collaborative Filtering (M2)** ทำผลงานได้ดีเป็นอันดับสอง (Recall@10 = `0.001524`) ใกล้เคียงกับ Baseline แต่ยังมีข้อจำกัดด้าน Coverage จากปัญหา Data Sparsity และ Cold-start
  - **Content-Based Filtering (M1 -> M1+):** การทดลองปรับปรุงโมเดลโดยเพิ่มข้อมูลส่วนผสม (`RecipeIngredientParts`) เข้าไปในกระบวนการ TF-IDF และ Cosine Similarity ช่วยเพิ่มค่า Recall@10 จาก `0.000333` เป็น `0.000400` และ Precision@10 จาก `0.000100` เป็น `0.000200` สะท้อนว่าข้อมูลส่วนผสมช่วยให้โมเดลแนะนำเมนูได้ตรงตามความชอบของผู้ใช้มากขึ้น
