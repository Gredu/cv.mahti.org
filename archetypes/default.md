---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
categories: ["item"]
type: "work"          # "work" or "volunteer" — which section it shows in
startYear: {{ now.Year }}
# endYear:            # optional; omit for a single year
# ongoing: true       # optional; renders "startYear ~" instead of an end year
# description: ""      # optional; faded text shown after the title
draft: true
---
