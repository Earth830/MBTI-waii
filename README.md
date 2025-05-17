<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>แบบทดสอบ MBTI</title>
  <style>
    body {
      font-family: "Segoe UI", sans-serif;
      padding: 20px;
      background: #f1f1f1;
    }
    .question-box {
      background: #fff;
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h2 {
      color: #333;
    }
    label {
      display: block;
      margin-bottom: 8px;
    }
    button {
      background-color: #3344aa;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background-color: #223388;
    }
    #result {
      margin-top: 30px;
      background-color: #e0ffe0;
      padding: 15px;
      border-radius: 10px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h2>แบบทดสอบบุคลิกภาพ MBTI</h2>
  <form id="mbtiForm"></form>
  <button onclick="submitTest()">ส่งคำตอบ</button>

  <div id="result"></div>

  <script>
    const form = document.getElementById("mbtiForm");

    const questions = [
      {
        text: "เมื่อเข้าสังคม คุณรู้สึกอย่างไร?",
        options: ["สนุกและมีพลัง", "รู้สึกเหนื่อยง่าย", "ตื่นเต้นกับผู้คนใหม่ๆ", "อยากรีบกลับบ้าน"],
        scores: ["E", "I", "E", "I"]
      },
      {
        text: "คุณมักตัดสินใจจากอะไร?",
        options: ["ความรู้สึกส่วนตัว", "ข้อมูลและเหตุผล", "สัญชาตญาณ", "คำแนะนำจากผู้อื่น"],
        scores: ["F", "T", "N", "S"]
      },
      {
        text: "เวลาวางแผนวันหยุด คุณจะ...",
        options: ["วางแผนล่วงหน้าละเอียด", "ไปแบบตามใจ", "จัดระเบียบอย่างเคร่งครัด", "เผื่อเวลาสำหรับการเปลี่ยนใจ"],
        scores: ["J", "P", "J", "P"]
      },
      {
        text: "คุณเชื่อใน...",
        options: ["หลักเหตุผล", "ความรู้สึก", "ประสบการณ์ตรง", "แนวคิดเชิงนามธรรม"],
        scores: ["T", "F", "S", "N"]
      },
      {
        text: "เวลาประชุมกลุ่ม คุณมักจะ...",
        options: ["พูดเปิดประเด็น", "สังเกตและฟัง", "กระตุ้นให้คนอื่นแสดงความคิดเห็น", "รอให้คนอื่นเริ่มก่อน"],
        scores: ["E", "I", "E", "I"]
      },
      {
        text: "คุณเชื่อว่าการเรียนรู้ที่ดีที่สุดคือ...",
        options: ["ลองทำจริง", "อ่านหนังสือ", "ฟังผู้อื่นอธิบาย", "สังเกตและวิเคราะห์"],
        scores: ["S", "N", "F", "T"]
      },
      {
        text: "เมื่อมีปัญหา คุณจะ...",
        options: ["หาทางแก้ทันที", "คิดก่อนแล้วค่อยทำ", "ขอความคิดเห็นคนอื่น", "ทำตามความรู้สึก"],
        scores: ["J", "P", "S", "F"]
      },
      {
        text: "เวลาเสนองานหรือไอเดีย...",
        options: ["ตรงไปตรงมาและชัดเจน", "ระมัดระวังคำพูด", "พูดน้อยและฟังมาก", "พูดจูงใจอย่างเป็นมิตร"],
        scores: ["T", "F", "I", "E"]
      },
      {
        text: "เมื่อเรียนรู้สิ่งใหม่ คุณชอบ...",
        options: ["ทำตามขั้นตอนอย่างละเอียด", "ทดลองทำจริง", "อ่านและวิเคราะห์ก่อน", "ถามคำถามเพื่อเข้าใจ"],
        scores: ["S", "N", "T", "F"]
      },
      {
        text: "คุณคิดว่าตัวเองเป็นคน...",
        options: ["ชอบวางแผนและจัดการทุกอย่าง", "ปรับตัวง่าย", "คิดลึกและใส่ใจรายละเอียด", "รักความสงบและไม่ชอบความวุ่นวาย"],
        scores: ["J", "P", "N", "I"]
      }
    ];

    questions.forEach((q, index) => {
      const box = document.createElement("div");
      box.className = "question-box";
      box.innerHTML = `<p><strong>${index + 1}. ${q.text}</strong></p>`;
      q.options.forEach((opt, i) => {
        box.innerHTML += `
          <label>
            <input type="radio" name="q${index}" value="${i}"> ${opt}
          </label>
        `;
      });
      form.appendChild(box);
    });

    function submitTest() {
      const formData = new FormData(form);

      for (let i = 0; i < questions.length; i++) {
        if (!formData.get("q" + i)) {
          alert("กรุณาตอบทุกคำถามก่อนส่ง");
          return;
        }
      }

      const scoreCount = {
        E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0
      };

      questions.forEach((q, i) => {
        const ansIndex = formData.get("q" + i);
        const dimension = q.scores[ansIndex];
        if (dimension) scoreCount[dimension]++;
      });

      const mbtiResult =
        (scoreCount.E >= scoreCount.I ? "E" : "I") +
        (scoreCount.S >= scoreCount.N ? "S" : "N") +
        (scoreCount.T >= scoreCount.F ? "T" : "F") +
        (scoreCount.J >= scoreCount.P ? "J" : "P");

      const descriptions = {
        "INTJ": "นักวางกลยุทธ์: วิเคราะห์เก่ง มีเป้าหมายชัดเจน มุ่งมั่นสูง",
        "INFP": "นักอุดมคติ: มีความเห็นอกเห็นใจ ช่างฝัน ยึดมั่นในค่านิยม",
        "ENTP": "นักสร้างสรรค์: ชอบความคิดใหม่ๆ มีพลังงานสูง และยืดหยุ่น",
        "ESFJ": "นักประสานงาน: เป็นมิตร ใส่ใจผู้อื่น และมีมนุษยสัมพันธ์ดี",
        "ISTP": "นักปฏิบัติ: มีเหตุผล ชอบลงมือทำจริง มักสงบเงียบ",
        "ENFP": "นักสร้างแรงบันดาลใจ: มองโลกในแง่ดี ชอบเข้าสังคม และมีจินตนาการ",
        "ISTJ": "นักตรวจสอบ: มีระเบียบ รอบคอบ และปฏิบัติตามกฎ",
        "ISFP": "ศิลปินผู้เงียบขรึม: อ่อนโยน มีความคิดสร้างสรรค์ รักอิสระ",
        // เพิ่มต่อได้อีก
      };

      let resultText = "ผลลัพธ์บุคลิกภาพ MBTI ของคุณคือ: " + mbtiResult + "\n\n";
      resultText += descriptions[mbtiResult] || "บุคลิกภาพ: " + mbtiResult + " (ยังไม่มีคำอธิบายเฉพาะ)";
      document.getElementById("result").innerText = resultText;
    }
  </script>
</body>
</html>
