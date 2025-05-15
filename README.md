# Uncle
GET BETTER 
<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>حاسبة السعرات </title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      direction: rtl;
      background: #f0f4f8;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 700px;
      margin: 50px auto;
      padding: 30px;
      background: #ffffff;
      border-radius: 15px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
      transition: 0.3s;
    }

    h2 {
      text-align: center;
      color: #2c3e50;
      margin-bottom: 30px;
    }

    label {
      font-weight: bold;
      margin-top: 15px;
      display: block;
      color: #34495e;
    }

    input, select {
      width: 100%;
      padding: 12px;
      border: 1px solid #ccc;
      border-radius: 10px;
      margin-top: 5px;
      background-color: #f9f9f9;
      box-sizing: border-box;
      font-size: 16px;
      transition: border 0.3s;
    }

    input:focus, select:focus {
      border-color: #3498db;
      outline: none;
    }

    button {
      margin-top: 25px;
      padding: 15px;
      width: 100%;
      background: #3498db;
      color: white;
      font-size: 18px;
      font-weight: bold;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #2980b9;
    }

    .result {
      margin-top: 30px;
      font-size: 18px;
      color: #2c3e50;
    }

    .meal {
      background: #ecf0f1;
      padding: 15px;
      border-radius: 10px;
      margin-top: 10px;
      font-size: 16px;
    }

    h3 {
      margin-top: 30px;
      color: #16a085;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>حاسبة السعرات واقتراح الوجبات</h2>

    <label>الهدف:</label>
    <select id="goal">
      <option value="maintain">الثبات</option>
      <option value="bulk">تضخيم</option>
      <option value="cut">تنشيف</option>
    </select>

    <label>الوزن (كغ):</label>
    <input type="number" id="weight" placeholder="مثال: 70">

    <label>الطول (سم):</label>
    <input type="number" id="height" placeholder="مثال: 175">

    <label>العمر (سنة):</label>
    <input type="number" id="age" placeholder="مثال: 25">

    <label>الجنس:</label>
    <select id="gender">
      <option value="male">ذكر</option>
      <option value="female">أنثى</option>
    </select>

    <label>مستوى النشاط:</label>
    <select id="activity">
      <option value="1.2">خامل (بدون نشاط)</option>
      <option value="1.375">نشاط خفيف</option>
      <option value="1.55">نشاط متوسط</option>
      <option value="1.725">نشاط عالي</option>
      <option value="1.9">نشاط شديد</option>
    </select>

    <button onclick="calculateCalories()">احسب السعرات والوجبات</button>

    <div class="result" id="result"></div>
    <div class="result" id="meals"></div>
  </div>

  <script>
    function calculateCalories() {
      const weight = parseFloat(document.getElementById('weight').value);
      const height = parseFloat(document.getElementById('height').value);
      const age = parseFloat(document.getElementById('age').value);
      const gender = document.getElementById('gender').value;
      const activity = parseFloat(document.getElementById('activity').value);
      const goal = document.getElementById('goal').value;

      if (!weight || !height || !age) {
        document.getElementById('result').innerText = "يرجى إدخال جميع البيانات.";
        return;
      }

      let bmr;
      if (gender === 'male') {
        bmr = 10 * weight + 6.25 * height - 5 * age + 5;
      } else {
        bmr = 10 * weight + 6.25 * height - 5 * age - 161;
      }

      let calories = bmr * activity;

      if (goal === 'bulk') {
        calories += 300;
      } else if (goal === 'cut') {
        calories -= 300;
      }

      calories = Math.round(calories);
      let goalText = goal === "bulk" ? " (تضخيم)" : goal === "cut" ? " (تنشيف)" : " (ثبات)";
      document.getElementById('result').innerText = `احتياجك اليومي: ${calories} سعرة حرارية ${goalText}`;
      suggestMeals(calories, goal);
    }

    function suggestMeals(calories, goal) {
      let meals = [];

      if (goal === 'bulk') {
        meals = [
          { name: "فطور: شوفان بالحليب + 3 بيضات + موزة", calories: 600 },
          { name: "سناك: تمر + زبدة فول سوداني", calories: 300 },
          { name: "غداء: دجاج مشوي + رز + خضار + زيت زيتون", calories: 800 },
          { name: "سناك: زبادي كامل الدسم + مكسرات", calories: 400 },
          { name: "عشاء: توست + تونة + أفوكادو", calories: 500 }
        ];
      } else if (goal === 'cut') {
        meals = [
          { name: "فطور: بيضتين + خيار + توست أسمر", calories: 300 },
          { name: "سناك: تفاحة + قهوة سوداء", calories: 100 },
          { name: "غداء: صدر دجاج + خضار مشوية + سلطة", calories: 500 },
          { name: "سناك: زبادي قليل الدسم", calories: 150 },
          { name: "عشاء: شوربة عدس + سلطة", calories: 250 }
        ];
      } else {
        meals = [
          { name: "فطور: 2 بيضة + توست + تفاحة", calories: 400 },
          { name: "سناك: مكسرات + زبادي", calories: 200 },
          { name: "غداء: دجاج + رز + خضار", calories: 600 },
          { name: "سناك: فاكهة موسمية", calories: 150 },
          { name: "عشاء: سلطة + تونة", calories: 300 }
        ];
      }

      let selected = [];
      let total = 0;

      for (let i = 0; i < meals.length && total + meals[i].calories <= calories; i++) {
        selected.push(meals[i]);
        total += meals[i].calories;
      }

      let html = `<h3>وجبات مقترحة تقارب ${total} سعرة:</h3>`;
      selected.forEach(meal => {
        html += `<div class="meal">${meal.name} - ${meal.calories} سعرة</div>`;
      });

      document.getElementById("meals").innerHTML = html;
    }
  </script>
</body>
</html>