# pandas-challenge
#import dependencies 
import pandas as pd
from pathlib import Path
#load in csv file paths
loadschooldata = Path("Resources/schools_complete.csv")
loadstudentdata = Path("Resources/students_complete.csv")
school_data = pd.read_csv(loadschooldata)
student_data = pd.read_csv(loadstudentdata)
#merge the two csv files
school_data_complete = pd.merge(student_data, school_data, how="left",on=["school_name","school_name"])
school_data_complete.head()
#the number of unique schools
school_count = school_data_complete["school_name"]
unique_school_count = school_count.unique()
total_unique_schools = len(unique_school_count)
total_unique_schools
#the total number of students
student_count = school_data_complete["student_name"].count()
student_count
#the total budget
total_budget = school_data["budget"].sum()
total_budget
#the average math score
average_math_school = school_data_complete["math_score"].mean()
average_math_school
#the average reading score
average_reading_score = school_data_complete["reading_score"].mean()
average_reading_score
#% of students who passed math
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]
passing_math_percentage = passing_math_count / float(student_count) * 100
passing_math_percentage
#% of students who passed reading
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]
passing_reading_percentage = passing_reading_count / float(student_count) * 100
passing_reading_percentage
#% of students who passed math and reading
passing_math_reading_count = school_data_complete[
    (school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
].count()["student_name"]
overall_passing_rate = passing_math_reading_count /  float(student_count) * 100
overall_passing_rate
#summary dataframe of the whole district
district_summary = {"Total Schools":[15],"Total Students":[39170],"Total Budget":[24649428],"Average Math Score":[78.985371],"Average Reading Score":[81.87784],"% Passing Math":[74.980853],"% Passing Reading":[85.805463], "% Overall Passing":[65.172326]}
district_summary = pd.DataFrame(district_summary)

district_summary["Total Students"] = district_summary["Total Students"].map("{:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)


district_summary
#find school type
school_types = school_data.set_index(["school_name"])["type"]
school_types
#calculated total student count per school
per_school_counts = school_data_complete["school_name"].value_counts()
per_school_counts
#total school budget and per capita spending
per_school_budget = school_data_complete.groupby(["school_name"])["budget"].mean()
per_school_capita = per_school_budget / per_school_counts
per_school_capita
#avg math scores per school
per_school_math = school_data_complete.groupby(["school_name"])["math_score"].mean()
per_school_math
#average reading score per school
per_school_reading = school_data_complete.groupby(["school_name"])["reading_score"].mean()
per_school_reading
#schools with passing math students
school_passing_math = school_data_complete[(school_data_complete["math_score"]>=70)]
school_passing_math
#schools with passing reading students
school_passing_reading = school_data_complete[(school_data_complete["reading_score"]>=70)]
school_passing_reading
#schools with students passing reading and math
passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
passing_math_and_reading
#schools with students passing reading and math
passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
passing_math_and_reading
#per school dataframe
per_school_summary = pd.DataFrame({
    "School Type": school_types,
    "Total Students": per_school_counts,
    "Total School Budget": per_school_budget,
    "Per Student Budget":per_school_capita,
    "Average Math Score": per_school_math,
    "Average Reading Score": per_school_reading,
    "% Passing Math": per_school_passing_math,
    "% Passing Reading": per_school_passing_reading,
    "% Overall Passing": overall_passing_rate})
per_school_summary
#the top 5 schools
top_5 = per_school_summary.sort_values(by='% Overall Passing', ascending=False).head(5)
top_5
#the bottom 5 schools
bottom_schools = per_school_summary.sort_values(by='% Overall Passing',ascending=True).head(5)
bottom_schools
#separate data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]
#here i got help from a friend in the next portion of code that finds the mean score of each grade
#group by school name and find the mean of each
ninth_graders_scores = ninth_graders.groupby(["school_name"]).mean()
ninth_graders_scores
tenth_graders_scores = tenth_graders.groupby(["school_name"]).mean()
tenth_graders_scores
eleventh_graders_scores = eleventh_graders.groupby(["school_name"]).mean()
eleventh_graders_scores
twelfth_graders_scores = twelfth_graders.groupby(["school_name"]).mean()
twelfth_graders_scores
#select only the math scores
ninth_grade_math_scores = ninth_graders_scores["math_score"]
tenth_grader_math_scores = tenth_graders_scores["math_score"]
eleventh_grader_math_scores = eleventh_graders_scores["math_score"]
twelfth_grader_math_scores = twelfth_graders_scores["math_score"]
#math score by grade dataframe
math_scores_by_grade = pd.DataFrame(
        {"9th": ninth_grade_math_scores, 
        "10th": tenth_grader_math_scores,
        "11th": eleventh_grader_math_scores, 
        "12th": twelfth_grader_math_scores})
math_scores_by_grade
#separate data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]
#group by school name and take the mean of each
ninth_graders_scores = ninth_graders.groupby(["school_name"]).mean()
ninth_graders_scores
tenth_graders_scores = tenth_graders.groupby(["school_name"]).mean()
tenth_graders_scores
eleventh_graders_scores = eleventh_graders.groupby(["school_name"]).mean()
eleventh_graders_scores
twelfth_graders_scores = twelfth_graders.groupby(["school_name"]).mean()
twelfth_graders_scores
#select just the reading score
ninth_grader_reading_scores = ninth_graders_scores["reading_score"]
tenth_grader_reading_scores = tenth_graders_scores["reading_score"]
eleventh_grader_reading_scores = eleventh_graders_scores["reading_score"]
twelfth_grader_reading_scores = twelfth_graders_scores["reading_score"]
#create the reading score by grade dataframe
reading_scores_by_grade = pd.DataFrame(
        {"9th": ninth_grader_reading_scores, 
        "10th": tenth_grader_reading_scores,
        "11th": eleventh_grader_reading_scores, 
        "12th": twelfth_grader_reading_scores})

reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None

reading_scores_by_grade
#establishing the bins
spending_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]
#creating a copy of the school summary to use "Per Student Budget"
school_spending_df = per_school_summary.copy()
#for the next line of code using pd.cut, i got help from an LA
#using pd.cut to categorize the spending 
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita,spending_bins,labels=labels)
school_spending_df
#calculating the averages for the proper columns
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Overall Passing"]
#creating the spending per student dataframe
spending_summary = pd.DataFrame({"Average Math Score":spending_math_scores,
                                    "Average Reading Score":spending_reading_scores,
                                    "% Passing Math":spending_passing_math,
                                    "% Passing Reading":spending_passing_reading,
                                    "% Overall Passing":overall_passing_spending})
spending_summary
#establishing the bins
size_bins = [0, 1000, 2000, 5000]
labelz = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
#using pd.cut on the Total Students column of per_school_summary dataframe
per_school_summary["School Size"] = pd.cut(per_school_summary["Total Students"],size_bins,labels=labelz)
per_school_summary
#calculating the averages for the columns
size_math_scores = per_school_summary.groupby(["School Size"]).mean()["Average Math Score"]
size_reading_scores = per_school_summary.groupby(["School Size"]).mean()["Average Reading Score"]
size_passing_math = per_school_summary.groupby(["School Size"]).mean()["% Passing Math"]
size_passing_reading = per_school_summary.groupby(["School Size"]).mean()["% Passing Reading"]
size_overall_passing = per_school_summary.groupby(["School Size"]).mean()["% Overall Passing"]
#create the size summary dataframe
size_summary = pd.DataFrame({"Average Math Score":size_math_scores,
                                    "Average Reading Score":size_reading_scores,
                                    "% Passing Math":size_passing_math,
                                    "% Passing Reading":size_passing_reading,
                                    "% Overall Passing":size_overall_passing})
size_summary
#grouping the per_school_summary DataFrame by "school type" and finding the average
type_math_scores = per_school_summary.groupby(["School Type"]).mean()
type_reading_scores = per_school_summary.groupby(["School Type"]).mean()
type_passing_math = per_school_summary.groupby(["School Type"]).mean()
type_passing_reading = per_school_summary.groupby(["School Type"]).mean()
type_overall_passing = per_school_summary.groupby(["School Type"]).mean()
#selecting the correct column data
average_math_score_by_type = type_math_scores["Average Math Score"]
average_reading_score_by_type = type_reading_scores["Average Reading Score"]
average_percent_passing_math_by_type = type_passing_math["% Passing Math"]
average_percent_passing_reading_by_type = type_passing_reading["% Passing Reading"]
average_percent_overall_passing_by_type = type_overall_passing["% Overall Passing"]
#creating the school type summary dataframe
type_summary = pd.DataFrame({"Average Math Score":average_math_score_by_type,
                "Average Reading Score":average_reading_score_by_type,
                "% Passing Math":average_percent_passing_math_by_type,
                "% Passing Reading":average_percent_passing_reading_by_type,
                "% Overall Passing Rate":average_percent_overall_passing_by_type})
type_summary
