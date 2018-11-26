## WSGI server
### waitress[win]
```
waitress-serve --port=8801 calculate.app:api
```

### gunicorn[linux]
```
gunicorn asistant:app
gunicorn --reload asistant.app

```

