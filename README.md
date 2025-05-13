import streamlit as st
import datetime
import os

# 建立儲存音檔資料夾
UPLOAD_DIR = "uploads"
if not os.path.exists(UPLOAD_DIR):
    os.makedirs(UPLOAD_DIR)

# 網頁標題
st.title("🎶 照小合唱團 練唱平台 2.0")
st.markdown("請依序填寫以下欄位並上傳您的錄音檔，系統會自動評分音準與節奏。")

# 班級輸入（需為三位數字）
class_input = st.text_input("請輸入班級（三位數字，如 501）：", max_chars=3)

# 姓名輸入
name_input = st.text_input("請輸入您的姓名：")

# 聲部選擇
voice_part = st.selectbox("請選擇聲部：", ["第一部", "第二部", "第三部"])

# 段落選擇
section_part = st.selectbox("請選擇練習段落：", ["前奏", "A段", "B段", "結尾"])

# 上傳音檔
uploaded_file = st.file_uploader("請上傳您的錄音檔（mp3 或 wav）：", type=["mp3", "wav"])

# 按鈕送出
if st.button("📤 上傳並評分"):
    if not all([class_input, name_input, uploaded_file]):
        st.warning("⚠️ 請確認已完整填寫所有欄位並選擇錄音檔案。")
    else:
        # 儲存檔案
        timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        filename = f"{class_input}_{name_input}_{voice_part}_{section_part}_{timestamp}.{uploaded_file.name.split('.')[-1]}"
        filepath = os.path.join(UPLOAD_DIR, filename)

        with open(filepath, "wb") as f:
            f.write(uploaded_file.getbuffer())

        st.success("✅ 上傳成功！")
        st.audio(uploaded_file, format="audio/mp3")

        # 簡易評分模擬（可後續接 AI 模型）
        import random
        pitch_score = random.randint(75, 100)
        rhythm_score = random.randint(70, 100)

        st.markdown(f"### 🎧 評分結果")
        st.write(f"🔸 音準分數：`{pitch_score} 分`")
        st.write(f"🔸 節奏分數：`{rhythm_score} 分`")
# zhao-chorus2.0
學生上傳音檔並自動評分的系統與教師後端監控學生學習狀況
