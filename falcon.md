## WSGI server
### waitress[win]
```
waitress-serve --port=8801 calculate.app:api
```

### gunicorn[linux]
```
gunicorn calculate.app
gunicorn --reload calculate.app

```

