# How to Install psycopg2 to Connect to Strata Scratch Using Python

You can install `psycopg2` in your Jupyter notebook. You'll only need to install `psycopg2` once and after that, 
you merely need to import `psycopg2`.

1. Open a new Jupyter notebook

2. In the notebook type the command below:

```
import sys
!conda install --yes --prefix {sys.prefix} psycopg2
```

