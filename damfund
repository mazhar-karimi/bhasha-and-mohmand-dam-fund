import matplotlib.pyplot as plt
import numpy as np

import requests
import re
import datetime


def best_fit(X, Y):

    xbar = sum(X)/len(X)
    ybar = sum(Y)/len(Y)
    n = len(X) # or len(Y)

    numer = sum([xi*yi for xi,yi in zip(X, Y)]) - n * xbar * ybar
    denum = sum([xi**2 for xi in X]) - n * xbar**2

    b = numer / denum
    a = ybar - b * xbar

    return a, b

URL = "http://scp.gov.pk/popup.aspx"

r = requests.get(url = URL)

content = r.content.decode('utf-8')

startscript_index = content.index('<script type')
endscript_index = content.index('</script>', startscript_index)

code =  str(content[startscript_index: endscript_index+9]).split('\n')
code_str = '\n'.join(code)

matcher = re.compile(r'data:\s\[[(\d+)\1*,?\s?]*\]')
m = matcher.findall(code_str, re.DOTALL)
d1 = m[0]

nums = re.findall(r'(\d+)',d1, re.DOTALL)

matcher = re.compile(r'labels:\s\[[(\"\d+/\d+/\d\")\1*,?\s?]*\]')
m = matcher.findall(code_str, re.DOTALL)
d1 = m[0]

X = re.findall(r'(\d+/\d+/\d+)',d1, re.DOTALL)

Y = None

Y = list(map(int, nums))

X = [datetime.datetime.strptime(i, "%d/%m/%Y").date() for i in X]

Y = [i/1000000 for i in Y]

X = X[:len(X)-1]
index = [i for i in range(1, len(X) + 1)]
plt.title('Bhasha Damn Funding')
plt.xlabel('Date')
plt.ylabel('Rs(millions)')
plt.plot(X, Y)

a, b = best_fit(index, Y)
yfit = [a + b * xi for xi in index]
plt.plot(X, yfit, color='red')

plt.show()

#m = re.match('\s*data\s*:\s*\[[\d+,?\s*]*\]', script, re.MULTILINE)
