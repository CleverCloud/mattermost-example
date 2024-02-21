# Deploy Mattermost on Clever Cloud

This repo is a collection of scripts to deploy [Mattermost](https://mattermost.com) on Clever Cloud using a [**Node.js**](https://developers.clever-cloud.com/doc/applications/javascript/nodejs/) app out of the box.

## Pre-requisites

In order to deploy this repo on Clever Cloud, you need the following:

- A [Clever Cloud account](https://developers.clever-cloud.com/doc/account/create-account/)
- A running database (either a [MySQL](https://developers.clever-cloud.com/doc/addons/mysql/[) or a [PostrgeSQL](https://developers.clever-cloud.com/doc/addons/postgresql/) add-on) on Clever Cloud
- A running [Cellar add-on](https://developers.clever-cloud.com/doc/addons/cellar/) for object (S3) storage on Clever Cloud

### Deploy Mattermost

Follow these steps to deploy Mattermost on Clever Cloud.

#### 1. 📂 Create a Cellar bucket

From the Clever Cloud console, go to your Cellar add-on's dashboard, enter the name of your bucket (it can be anything) and click on *Create bucket**.

![Create bucket option on Clever Cloud Console](/assets/cellar-bucket-create.png)

#### 2. ☁️ Create a Node.js app

Mattermost is usually configured with a `config.json` file, but you can override this file by injecting environment variables. On Clever Cloud, **environment variables are injected dynamically**, the following steps give you the ones you need to deploy Mattermost.

Check both your database add-on and your Cellar addon dashboard in the Console to get the environement variables' values.

1. Create a new Node.js app
2. Follow the creation tunnel until you get to the environment variables
3. Set the following environment variables

- `MATTERMOST_VERSION="<your_desired_version>"`
- The port to listen to : `MM_SERVICESETTINGS_LISTENADDRESS=":8080"`
- Database connection string: Replace `< >` statement with the appropiate URI from your database environment variable:
  - For PostrgeSQL: `MM_SQLSETTINGS_DATASOURCE="<POSTGRESQL_ADDON_URI_value>?sslmode=disable&connect_timeout=10`
  - For MySQL: `MM_SQLSETTINGS_DATASOURCE=<MYSQL_ADDON_URI_value>?charset=utf8mb4,utf8&writeTimeout=30s"`
  - Database driver: `MM_SQLSETTINGS_DRIVERNAME="postgres"` or `"mysql"`
  - The maximum open connections allowed to your database: `MM_SQLSETTINGS_MAXOPENCONNS="<maximum_connections_to_db>"` (find it in **Add-on dashboard > Information > "Features"** section)
- Your S3 credentials:

```shell
MM_FILESETTINGS_AMAZONS3ACCESSKEYID="<cellar_Key_ID>"
MM_FILESETTINGS_AMAZONS3BUCKET="<bucket_name>"
MM_FILESETTINGS_AMAZONS3ENDPOINT="cellar-c2.services.clever-cloud.com"
MM_FILESETTINGS_AMAZONS3SECRETACCESSKEY="<cellar_Key_Secret>"
MM_FILESETTINGS_DRIVERNAME="amazons3"
```

#### 3. 🚀 Deploy the app

After configuring your variables, you are ready to push this repo to Clever Cloud. Once the deployment finishes, your Mattermost app stays connected to your database and will store assets in Cellar.

**📌 Note**: Since you are injecting the database and Cellar credentials in the environment variables, you don't need to connect the app to the add-ons in the console.

**🎓 More questions on deployments?** Read [our doc](https://developers.clever-cloud.com).
