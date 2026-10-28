[PUMP_Gym_OS_Azure_Cloud_Documentation .docx](https://github.com/user-attachments/files/34082749/PUMP_Gym_OS_Azure_Cloud_Documentation.docx)

```
Flutter Web (pump-web, Static Web Apps Free)
        |  HTTPS + JSON, WebSocket (SignalR)
        v
ASP.NET Core API (pump-api, App Service F1)
   |-- Azure SQL Database (pump-db, Free Offer, serverless)
   |-- Blob Storage (pumpstorage: avatars, exports)
   |-- Azure SignalR Service (pump-signalr, Free)
   |-- Key Vault (pump-vault) via managed identity
   '-- Application Insights (pump-insights)
