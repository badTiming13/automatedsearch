import json
import pandas as pd
import requests

###correct
lines = []

f = open("specimens.txt")
lines = f.read().splitlines()
f.close()
print(lines)


with open('readme.json', 'wb') as f:
    response = requests.post('http://YOUR_ENDPOINT', json={"Auftragsnummern": lines})
    f.write(response.content)


######

##imports for correct functioning on Server
import openpyxl
import xlsxwriter



#Modifying JSON to the right format
line_num = 0
x = 0

with open("readme.json", "r") as f:
    x = len(f.readlines())
    print(x)

with open("readme.json", "rt") as fin:
    with open("out.txt", "wt") as fout:
        for line in fin:
            fout.write(line.replace('''"Auftraege":''', ''))

with open("out.txt") as f:
    lines = f.readlines()
    lines[0] = "["
    lines[-1] = "]"
    print(lines)
    with open("out.txt", "r") as file:
        with open("out.json", "w") as fout:
            for line in lines:
                fout.write(line)

#Convertion of JSON to CSV

def export_to_csv():
    with open("out.json") as f:
        list1 = []
        data = json.loads(f.read())
        temp = data[0]
        header_items = []
        get_header_items(header_items, temp)
        list1.append(header_items)

        for obj in data:
            d = []
            add_items_to_data(d, obj)
            list1.append(d)

        with open('output.csv', 'w') as output_file:
            for a in list1:
                output_file.write(','.join(map(str, a)) + "\r")


def get_header_items(items, obj):
    for x in obj:
        if isinstance(obj[x], dict):
            items.append(x)
            get_header_items(items, obj[x])
        else:
            items.append(x)


def add_items_to_data(items, obj):
    for x in obj:
        if isinstance(obj[x], dict):
            items.append("")
            add_items_to_data(items, obj[x])
        else:
            items.append(obj[x])


export_to_csv()

csv = pd.read_csv('output.csv')

excelWriter = pd.ExcelWriter('new.xlsx')

csv.to_excel(excelWriter, sheet_name='auftraege', index=False)

for column in csv:
    column_width = max(csv[column].astype(str).map(len).max(), len(column))
    col_idx = csv.columns.get_loc(column)
    excelWriter.sheets['auftraege'].set_column(col_idx, col_idx, column_width)

excelWriter.save()
