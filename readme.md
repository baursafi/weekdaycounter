Hi, 

here is the code for anyone who was asked to count the number of weekdays (Mondays, Tuesdays, and so on) in a month of any year. 
Feel free to tweek it and / or maybe use it for your own tasks. 

'''
Initial idea stems from a request I got from my boss, and looking for ready solutions I found this function, 
that I slightly remodeled based on the correctness of output and the business task.
https://stackoverflow.com/questions/43692340/how-to-find-number-of-mondays-or-any-other-weekday-between-two-dates-in-python/43692481
'''

def weekday_count(start, end):
    start_date  = datetime.strptime(start, '%d/%m/%Y')
    end_date    = datetime.strptime(end, '%d/%m/%Y')
    week        = {}
    for i in range((end_date - start_date).days+1):
        day = calendar.day_name[(start_date + timedelta(days=i)).weekday()]
        week[day] = week[day] + 1 if day in week else 1
    week['year'] = str((start_date+timedelta(days=i)).year)
    week['month'] = str((start_date).month)
    mo = (week['month'],'0'+week['month'])[len(week['month'])<2]
    week['yearmo'] = week['year'] + mo

    return week

table = pd.DataFrame()
# year = int(input('Please enter the year\n you want to know the number of weekdays in:  '))
for year in [2016,2017, 2018, 2019, 2020]:
    for month in range(1, 13):
        start = datetime.strptime(('1/'+str(month)+"/"+str(year)), '%d/%m/%Y').strftime('%d/%m/%Y')

        if month == 12:
            end = (datetime.strptime(('1/1/'+str(year+1)), '%d/%m/%Y')-timedelta(days=1)).strftime('%d/%m/%Y')
        else:
            end = (datetime.strptime(('1/'+str(month+1)+"/"+str(year)), '%d/%m/%Y')- timedelta(days=1)).strftime('%d/%m/%Y')
        #print('st-',start,'//en-',end)
        table = table.append(weekday_count(start, end), ignore_index = True)
filename = 'weekday_counter.csv'
table[['year', 'month', 'yearmo',
       'Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday']].to_csv(filename, index = False, encoding='utf-8-sig')

os.startfile(filename)
