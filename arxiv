#!/usr/bin/python3
# encoding=utf8

import os, re, sys, subprocess
import urllib.request as urllib2
import urllib.parse
from bs4 import BeautifulSoup

version = 1.0

arguments = {}
arguments['-h, --help'] = 'Print help'
arguments['-v, --version'] = 'Print Version'

# ================== Settings ====================

url = "https://arxiv.org/list/astro-ph/new"

# ================================================

class color:
   PURPLE = '\033[95m'
   CYAN = '\033[96m'
   DARKCYAN = '\033[36m'
   BLUE = '\033[94m'
   GREEN = '\033[92m'
   YELLOW = '\033[93m'
   RED = '\033[91m'
   BOLD = '\033[1m'
   UNDERLINE = '\033[4m'
   END = '\033[0m'

if __name__ == "__main__":

  # =============== Argument parser=================

  if any([1 if arg in sys.argv else 0 for arg in ['-v', '--version']]):
      print(version)
      sys.exit(0)

  if any([1 if arg in sys.argv else 0 for arg in ['-h', '--help']]):

    name = os.path.basename(sys.argv[0])

    # Display help
    print("This is {program}. Get your daily arXiv-dose.\n".format(program=name))
    print("Usage: ./{program}".format(program=name))
    print("Currently I'm fetching", url, '\n')

    for key in arguments:
        print("\t{:15}: {}".format(key, arguments[key]))

    sys.exit(0)

  # ================================================

  # ============ Generate and fetch url ============


  try:
    req = urllib2.Request(url, headers={'User-Agent': 'Mozilla/5.0'})
    html = urllib2.urlopen(req)

  except urllib2.HTTPError:
    print(url)
    print('"{}" not found. Correct spelling?'.format(search))
    sys.exit(0)

  # ================================================

  # ================= Find papers ==================

  soup = BeautifulSoup(html, "lxml")

  articles = {}

  # Get DOI and URL
  papers = soup.find_all("dt")

  for c, nnn in zip( papers, range( len(papers) ) ):

    articles[nnn] = {}

    doi = c.find_all("a", title="Abstract")[0]
    doi = doi.get_text()
    articles[nnn]["doi"] = doi

    link = c.find_all("a", title="Download PDF")[0].get("href")
    articles[nnn]["url"] = 'https://arxiv.org' + link

  # Get Title, Authors and Abstract
  meta = soup.find_all("div", class_="meta")

  for c, nnn in zip(meta, range(len(meta))):

    title = c.find("div", class_="list-title")
    title = title.get_text().replace('Title: ','')
    articles[nnn]["title"] = title.strip()

    authors = c.find("div", class_="list-authors")
    authors = authors.get_text().replace('Authors:','').replace('\n','')
    authors = re.sub('[a-zA-Z]+\.+\ ','',authors)
    articles[nnn]["authors"] = authors.strip()

    try:
      abstract = c.find("p", class_="mathjax").get_text().replace('\n',' ')
    except AttributeError:
      pass

    articles[nnn]["abstract"] = abstract

  # List findings
  for paper in articles.keys():

    print( '\n' + color.BOLD + color.UNDERLINE +'{:5}'.format(paper) + color.END,
           articles[paper]["title"])
    print( 6 * ' ' + articles[paper]["authors"], '\n' )
    print( ' ' + articles[paper]["abstract"] )

  # Get user input list
  while True:

    download = input( '\n' + color.BOLD + 'Download (2 12 ..): ' + color.END )

    try:
      download = [ int(i) for i in download.split() ]
      break

    except ValueError:
      print('Not a valid list: "{}"'.format(download))
      pass

  for file in download:

    url = articles[file]["url"]
    filename = '{}-{}-{}.pdf'.format(articles[file]["title"], articles[file]["authors"], articles[file]["doi"])

    # EXT4 limits filenames to 255 characters

    if len(filename) > 254:

      filename = articles[file]["title"] + '-'

      for author in articles[file]["authors"].split():
        if len(author) + len(filename) + len(articles[file]["doi"]) + 5 < 255:
          filename += author.strip()

      filename = filename[:-1] + '-' + articles[file]["doi"] + ".pdf"
      print(color.BOLD + 'Warning:' + color.END + 'Too many authors for |filename| < 256.')
      print('Truncating to ', filename)

    # Download
    subprocess.call(["wget", '--quiet', '--show-progress', '--header', "User-Agent: Mozilla/5.0", "--output-document", '{}'.format(filename), url])

  # ================================================
