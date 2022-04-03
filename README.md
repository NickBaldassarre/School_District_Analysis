# An Analysis of School and Student Performance

## Project Overview

### I will be assisting a city school district with an analysis of its schools. We will be looking at the relationships between school budgets, school sizes, and academic performance. We will also be removing grades that are suspect, and determining what changes occur to our final analysis.

## Analysis

In order to obtain more accurate data, we removed data that was suspected of academic dishonesty. We removed all grades from the 9th grade at Thomas High School by using the .loc method along with comparison operators to retrieve all rows that satisfied both conditions. We also imported NumPy in order to use the nan method to change all math and reading scores that satistfied both conditions to NaN.

    student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"),["reading_score"]] = np.nan
    student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"),["math_score"]] = np.nan

We confirmed the NaN conversion by using the isnull() method chained with the sum() method to count the number of instances of null values. The null values were confirmed in both math and reading scores.

Now that we had removed the data that was suspect, we were able to run new calculations based on a new total number of students. We created a new District Summary DataFrame that took into account these new student totals when running our calculations.

    # Step 1. Get the number of students that are in ninth grade at Thomas High School.
    # These students have no grades. 
    thomas_9th_count = len(school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["grade"] == "9th")])
    
    # Step 2. Subtract the number of students that are in ninth grade at 
    # Thomas High School from the total student count to get the new total student count.
    new_student_count = student_count - thomas_9th_count
    
    # Step 3. Calculate the passing percentages with the new total student count.
    passing_math_percentage = passing_math_count / float(new_student_count) * 100
    passing_reading_percentage = passing_reading_count / float(new_student_count) * 100
    
    # Step 4.Calculate the overall passing percentage with new total student count.
    overall_passing_percentage = overall_passing_math_reading_count / new_student_count * 100

![District Summary](https://github.com/NickBaldassarre/School_District_Analysis/blob/6d02874c4ea07c7012c4e2df60513a1b5c7c08fb/resources/district_summary.png)

The change in these numbers compared to our first analysis is negligible, with slight drops of one to two 10ths of a percent. This is consistent with removing grades that may have been higher than they should have been.

The change is slightly more apparent at the school level. Thomas High School's reading and math scores were higher when we were including the 9th grade scores. Below we can see the scores before, then after the removal of 9th grade data.

![Thomas High School Before Removal](https://github.com/NickBaldassarre/School_District_Analysis/blob/6d02874c4ea07c7012c4e2df60513a1b5c7c08fb/resources/top_five_before_purge.png)
![Thomas High School After Removal](https://github.com/NickBaldassarre/School_District_Analysis/blob/6d02874c4ea07c7012c4e2df60513a1b5c7c08fb/resources/top_five_adjusted.png)

We then seperated the data by grade level using conditions and the groupby() method.

    # Create a Series of scores by grade levels using conditionals.
    ninth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "9th")]
    tenth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "10th")]
    eleventh_graders = school_data_complete_df[(school_data_complete_df["grade"] == "11th")]
    twelfth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "12th")]

    # Group each school Series by the school name for the average math score.
    ninth_grade_math_scores = ninth_graders.groupby(["school_name"]).mean()["math_score"]
    tenth_grade_math_scores = tenth_graders.groupby(["school_name"]).mean()["math_score"]
    eleventh_grade_math_scores = eleventh_graders.groupby(["school_name"]).mean()["math_score"]
    twelfth_grade_math_scores = twelfth_graders.groupby(["school_name"]).mean()["math_score"]

    # Group each school Series by the school name for the average reading score.
    ninth_grade_reading_scores = ninth_graders.groupby(["school_name"]).mean()["reading_score"]
    tenth_grade_reading_scores = tenth_graders.groupby(["school_name"]).mean()["reading_score"]
    eleventh_grade_reading_scores = eleventh_graders.groupby(["school_name"]).mean()["reading_score"]
    twelfth_grade_reading_scores = twelfth_graders.groupby(["school_name"]).mean()["reading_score"]
    
We used this data to create new DataFrames which show each grade level in each school. These DataFrames confirm that all 9th grade values at Thomas High School are NaN.

![Math Averages by Grade](https://github.com/NickBaldassarre/School_District_Analysis/blob/97b1fcbc0238979da0c8ea2910a093155789bf41/resources/math_averages_by_grade.png)
![Reading Averages by Grade](https://github.com/NickBaldassarre/School_District_Analysis/blob/97b1fcbc0238979da0c8ea2910a093155789bf41/resources/reading_averages_by_grade.png)

## Results

* The District Summary shows a slight dip in average scores after the removal of the data.
* The School Summary shows a slight decrease in the scores from Thomas High School
* Replacing the 9th grade scores from Thomas High School does not change Thomas's position relative to other schools as the chamge is negligible.
* Math and reading scores by grade are affected only in that Thomas shows NaN for 9th grade scores.
* Scores by school spending see slightly a slight drop in the $631-$645 per student bin as that is the bin that Thomas is in.
* Scores by school size see slightly a slight drop in the Medium (1000-1999) bin as that is the bin that Thomas is in.
* Scores by school type see slightly a slight drop in the Charter bin as that is the bin that Thomas is in.

## Summary

We can see that replacing the 9th grade scores from Thomas High School does alter the final analysis, but only slightly. One change is that the reading and math scores are slightly lower for Thomas High School after the replacement. Another change is in the District Summary which shows slightly lower grade averages overall. There is also a change in the $632-$645 school bin, with a slight decrease in grade scores. A fourth change is in the Medium bin in which Thomas High School resides, again with a slight decrease in grade scores.

It should be noted that the changes were so negligible that they were not apparent at first. That being said, the 9th grade scores did bring the averages for Thomas High School up slightly, so they may have been artificially high. It should also be noted that the amount of money spent per student was not as important as the school size or type. Charter schools scored far better than district schools, even though many spent less per student that the lowest performing schools. Charter schools are also generally smaller than district schools, meaning that not only are they spending less per student, but their total budgets are considerably less than district schools. 

![Scores by School Size](https://github.com/NickBaldassarre/School_District_Analysis/blob/a115cb7f9b5a0042a66d4e776ac8fb7be1579b46/resources/school_size.png)
![Scores by School Spending](https://github.com/NickBaldassarre/School_District_Analysis/blob/a115cb7f9b5a0042a66d4e776ac8fb7be1579b46/resources/spending_ranges.png)
![Scores by School Type](https://github.com/NickBaldassarre/School_District_Analysis/blob/a115cb7f9b5a0042a66d4e776ac8fb7be1579b46/resources/school_type.png)

It seems that the more students there are in a school, the more they cost, and the poorer they perform. Reducing the size of district schools would potentially help to increase performance.
