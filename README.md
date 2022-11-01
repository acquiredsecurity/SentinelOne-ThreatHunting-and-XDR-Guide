# SentinelOne-Threat-Hunting-Guide
How to Hunt the Threats

Hypothesis - Tune/Review - Evaluate/Analysis

Beginners Tips/Guide to Hunting for Threats

Threat Hunting / searching for behaviors in your environment that may be associated with malicious activity is a great way to learn more about your environments or your customers' environments, so you can explore and assess what is normal and what is suspicious/malicious. This is a simple guide to show users of the XDR Platform SentinelOne how to learn some basic threat hunting concepts.

Here are the basic steps I follow when performing hunting with SentinelOne. For this guide I am going to provide a simple example: The execution of powershell where an external DNS request is made. CmdLine Contains Anycase "powershell" AND DnsRequest EXISTS

Tips before we get started:

-- You're a threat hunter and you are looking for the proverbial needle in the haystacks. 
-- A good hunting query may return +100K's of results and you might be interested in one line out of those hundreds of thousaands of lines. 
-- Cast a wide net and work backwards, if you knew what you were looking for you could write a query to find it but you don't!
-- How to get good at this job? Look at a lot of results from your queries. Learn what's normal so you can find what's abnormal.
      -- Document your results in confluence or a shared resource across your team. 
      -- Work with team members who might be more experienced and ask questions.


1. Hypothesis - Determine which query or sets of queries I would like to run and what my time frame / scope is for performing a hunt.

Be sure to set your Scope/Time Frame and your max results. I personally like to set my results to max 20K and time frame to 14 days to see what I get when I am running a query for the first time. This will help determine how one might want to adjust their hunt once it is determined how many results are returned. If the query you run maxes out at 20K you will want to tune it and remove false positives until you get some result less than 20K, unless you determine you have 20K malicious results =(. Just keep in mind that if you are maxing out at 20K results you aren't getting the full picture in your query response (as only the first 20K results are returned) and need to tune out false positives or adjust your time frame. 


2. Tune/Review - Work to eliminate false positives from your results. If the query above returned the max of 20K results there will definitely be tuning required.

The following are steps I take to tune/analyze queries.

a. Export results to CSV with all columns 

b. Run a macro in excel to remove all columns that don't contain any data (Included in this module) 

c. Reformat your date columns, save your CSV as an xlsx, lock the top rows and use the data function to filter. Sort columns of importance to group similar things together as needed. 

d. Build a pivot table in excel to analyze my result. (In the example above I am interested in two main data sources. Data that contains command line information and the DNSRequest information. If I determine I have 20K dns requests to microsoft.com I may need to adjust my query to something like: CmdLine Contains Anycase "powershell" AND DnsRequest EXISTS and DnsRequest Does Not ContainCIS "microsoft.com") 

e. Use this data export to see how data is stored in SentinelOne. In my experience things change quite frequently unnanounced, so the more you hunt the more you may notice new pieces of info that can help in your hunting.



3. Evaluate/Analysis - Analyze the results of your query by enumerating processes/data points, pivoting on suspicious indicators and using external intelligence to review and verify. There may be some overlap between this step and step 2 Tune/Review. 

a. For the example search I would look at the command line information and domains from the results and determine if this is expected behavior or if it's potentially malicious.

Command Line - simply evaluating what the source process/script is, where it's executing from and if that's expected behavior might be enough to let me know if I need to escalate and investigate. Using the file fetch capability in SentinelOne to pull back the script or binary and evaluate it further in a malware sandbox or through other static/dynamic analysis techniques. Maybe just reviewing the command line itself will tell me what's going on what's the command line tell me it's doing?

Domains - Create a pivot table on the domains and eliminate the known good and further analyze the suspicious looking domains with OSINT tools like Virus Total.

                                          
   THE END                                       
  
  
 

 
 


