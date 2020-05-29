Use TCMapsTempOSim.tex as the latex file with a TemplateParameters.tex
file created as normal to create a PDF.

use `pdfjam` to create separate PDF files for each page.

use `pdfcrop` to crop away the white space

use ImageMagick's convert to make these into PNGs.

This hacky Python does it and gets the filenames right for Libor uploads

~~~Python
f = "TCMapsTempOSim.pdf"
num_stations = 1
num_problems = 6
base = 1

fname = "tmp.pdf"
cname = "tmp-crop.pdf"

for s in range(1, num_stations+1):
  for p in range(1, num_problems+1):
    page = base + (s-1) * num_problems + (p-1)
    print("gs -dNOPAUSE -dBATCH -sOutputFile=%s -dFirstPage=%d -dLastPage=%d -sDEVICE=pdfwrite %s" % (fname, page, page, f))
    print("pdfcrop %s" % fname)
    outfile = "map-%d.%dz.png" % (s, p)
    print("convert -density 150 -quality 100 %s %s" % (cname, outfile))
print("rm %s %s" %(cname, fname))
~~~

