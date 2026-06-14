import streamlit as st
import pandas as pd
from datetime import date

st.set_page_config(page_title="AI Smart Study Planner", page_icon="📚")

st.title("📚 AI Smart Study Planner")
st.write("Plan your studies smartly and generate a personalized timetable.")

# Student Details
st.header("Student Information")
name = st.text_input("Student Name")
study_hours = st.slider("Available Study Hours Per Day", 1, 12, 4)

# Exam Details
st.header("Add Exam")

subject = st.text_input("Subject Name")
exam_date = st.date_input("Exam Date", min_value=date.today())
difficulty = st.selectbox("Difficulty Level", ["Easy", "Medium", "Hard"])

if "exams" not in st.session_state:
    st.session_state.exams = []

if st.button("Add Exam"):
    st.session_state.exams.append({
        "Subject": subject,
        "Exam Date": exam_date,
        "Difficulty": difficulty
    })
    st.success("Exam Added Successfully!")

# Display Exams
if st.session_state.exams:
    st.subheader("Exam List")
    df = pd.DataFrame(st.session_state.exams)
    st.dataframe(df)

# Generate Timetable
if st.button("Generate Study Plan"):
    if len(st.session_state.exams) == 0:
        st.warning("Please add at least one exam.")
    else:
        st.subheader("📅 Personalized Study Plan")

        for exam in st.session_state.exams:
            if exam["Difficulty"] == "Hard":
                hours = study_hours * 0.5
            elif exam["Difficulty"] == "Medium":
                hours = study_hours * 0.3
            else:
                hours = study_hours * 0.2

            st.write(
                f"✅ **{exam['Subject']}** "
                f"({exam['Difficulty']}) → "
                f"Study **{hours:.1f} hrs/day**"
            )

# Progress Tracker
st.header("📈 Study Progress")
progress = st.slider("Completion Percentage", 0, 100, 0)
st.progress(progress)

if progress == 100:
    st.success("🎉 Congratulations! Your preparation is complete.")

# Footer
st.markdown("---")
st.write("Developed with Streamlit ❤️")vv
