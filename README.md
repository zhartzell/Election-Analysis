# Election-Analysis

## Overview of Election Audit: 
The purpose of this analysis is to compile and count all of the votes from three different Colorado counties in order to determine who won the congressional election. In addition, the team is interested in generating some statistics from the vote so that the client has a better understanding of how each county voted individually. 

## Election-Audit Results: 

### Total votes cast in the congressional election: 
 In the congressional election, there were 369,711 votes cast. 
 
### Breakdown of the number of votes and the percentage of total votes for each county in the precinct:
 The three counties in this analysis are Jefferson, Denver, and Arapahoe. Below is the breakdown for each county. 
 **Jefferson:** 10.5% (38,855)
 **Denver:** 82.8% (306,055)
 **Araphoe:** 6.7% (24,801)
 
### The county with the largest number of votes cast: 
Denver was the county with the largest number of votes. 

### Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
There were three candidates in this election. Below is a breakdown of how the votes tallied up for each candidate. 
**Charles Casper Stockham:** 23.0% (85,213)
**Diana DeGette:** 73.8% (272,892)
**Ramon Anthony Doane:** 3.1% (11,606)

### The winning candidate, the total votes they recieved, and the percentage of votes they recieved: 
**Winning Candidate:** Diana DeGette
**Winning Vote Count:** 272,892
**Winning Percentage:** 738.8%

### Election-Audit Summary: 
The code displayed below can be used to analyze future election data that is provided in a CVS file. Because magic numbers were avoided, this code can be easily transfered to a wide variety of elections with a varying assortment of data. If additional statistical information is desired, this code can be tweaked in order to produce that information. For example, compiling the data with past data to determine voting trends or the distribution of votes per demographic group (age, gender, political party affiliation). 

# Congressional Election Analysis Code: 

# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

Add our dependencies.
import csv
import os

Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")

Add a variable to save the file to a path.
file_to_save = os.path.join("Resources", "analysis", "election_analysis.txt")

Initialize a total vote counter.
total_votes = 0

Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}


Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

Track the largest county and county voter turnout.
largest_turnout_county = ""
largest_turnout_county_vote_count = 0
largest_turnout_county_vote_percentage = 0

Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:

            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1


Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in county_votes: 
        # 6b: Retrieve the county vote count.
        county_vote_count = county_votes.get(county_name)
        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(county_vote_count) / float(total_votes) * 100

         # 6d: Print the county results to the terminal.
        county_vote_results = (f"{county_name}: {county_vote_percentage: .1f}% ({county_vote_count:,}) \n")
        print(county_vote_results)
         # 6e: Save the county votes to a text file.
        txt_file.write(election_results)
         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote_count > largest_turnout_county_vote_count):
            largest_turnout_county_vote_count = county_vote_count
            largest_turnout_county = county_name
            largest_turnout_county_vote_percentage = county_vote_percentage

    # 7: Print the county with the largest turnout to the terminal.
    largest_county_turnout_summary = (f"------------------"
        f"County with the largest turnout: {largest_turnout_county}\n"
        f"Vote Count: {largest_turnout_county_vote_count}\n"
        f"Percentage: {largest_turnout_county_vote_percentage}\n"
        f"------------------\n")
    print(largest_county_turnout_summary)
    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_turnout_summary)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
