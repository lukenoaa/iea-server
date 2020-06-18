# erddap-config
The primary ERDDAP config files are `setup.xml` and `datasets.xml`.
An example of these files is provided.
These files should be modified and committed to your git repository.

Data hosted by ERDDAP is generally too large to save in git.
Data should be added as a docker volume using `compose.yml`.

## Setting up ERDDAP
2. set `baseUrl` & `baseHttpsUrl` in `erddap/setup.xml`
    * should be `localhost` or your server hostname
4. set up ERDDAP data sources and `datasets.xml` (see below)


### Modifying for your datasets
Data can be added to ERDDAP in two ways:

1. add ERDDAP data hosted by another ERDDAP server
2. add raw data to ERDDAP on (or mounted on) your docker host machine

#### (1) Adding data hosted by another ERDDAP server
This essentially set up your ERDDAP as a proxy for another ERDDAP.
This is useful for organizational purposes, but also because the remote ERDDAP server may not support CORS which is needed for the grafana-erddap plugin.
All that is required is to add a section to `erddap/content/datasets.xml` resembling the following:

```xml
<dataset type="EDDGridFromErddap" datasetID="jplMURSST41anom1day" active="true">
    <sourceUrl>https://coastwatch.pfeg.noaa.gov/erddap/griddap/jplMURSST41anom1day</sourceUrl>
</dataset>
```

#### (2) Adding raw local data to ERDDAP  
1. modify `docker-compose.yml:erddap:volumes` to list your data directories.
2. modify `erddap/datasets.xml` to describe your datasets.

## troubleshooting
1. check `http://localhost:8080/erddap/status.html` for issues.
2. test loading a specific dataset:
    * `sudo docker exec -it erddap bash -c "cd webapps/erddap/WEB-INF/ && bash DasDds.sh"`
