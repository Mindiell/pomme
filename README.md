<div style="text-align:center">
    <img src="https://github.com/JulienPalard/tuw/blob/master/assets/logo/logo.png?raw=true" alt="Pomme logo"/>
</div>
<!-- TODO: Fix centered image -->

# Pomme
#### Po Memory Database for open source documentation translators

To run it, use the following scripts, in that precise order:

- `fetch.py`: To crawl github for `po` files.
- `po_to_json.py github.com` to parse and aggregate po files.
- `index.py` to create an index from the previous aggregate.
- `FLASK_APP=search flask run` to run the service.

Then you can query the service like this:

```
$ curl -s http://localhost:8000/search/?q=socket | json_pp | head
{
   "\n%s: -w option cannot use a relative socket directory specification\n" : {
      "\n%s : l'option -w ne peut pas utiliser un chemin relatif vers le répertoire de\nla socket\n" : {
         "seen" : 1,
         "sources" : {
            "github.com/woonhak/postgresql-directio/src/bin/pg_ctl/po/fr.po" : 1
         }
      },
   ...
```

## Celery workers

### Run the workers

```
celery -A pomme worker -E --loglevel=INFO -B --concurrency=1
```

### Call the task

```
celery -A pomme call pomme.tasks.crawl_github.index_python_docs_fr
```

Apple icon made by <a href="https://www.flaticon.com/free-icon/apple_3616363?term=apple&page=1&position=27" title="DinosoftLabs">DinosoftLabs</a> from <a href="https://www.flaticon.com/" title="Flaticon"> www.flaticon.com</a>