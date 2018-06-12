# OpenHab Setup
This tutorial does a quick explanation on how to send the data from EmonCMS over MQTT to be read by OpenHab and display a simple statistic on power usage/generation. This setup requires you to have Persistence on OpenHab with MQTT. 

## EmonCMS Input process 
In order for OpenHab to be able to read the MQTT messages we have to send them first with EmonCMS. Choose your input and add a process setup like this: 

![Image of EmonCMS MQTT Process](https://i.imgur.com/cZQnjcB.png "EmonCMS MQTT Process")

## OpenHab Items
```
Number  ct1power          "Realtime [%.1f W]" {mqtt="<[mosquitto:emoncms/ct1/power:state:default]"}
Number  ct1powerday       "Today [%.1f kWh]"
Number  ct1powerhour      "Last hour [%.1f kWh]"
Number  ct1powercum       "Total [%.2f kWh]"   {mqtt=">[mosquitto:emoncms/ct1/powercum:state:*:default]"}
```

## OpenHab Rules
The `Energy Init` Rule will make sure the values exist at the start. It can be removed after a first boot.
```
var long LastUpdate = 0
////////////////////////////////////////////////////////////////////////////////////////////////
rule "Energy init"
////////////////////////////////////////////////////////////////////////////////////////////////
when
System started
then
        postUpdate(ct1powercum, 0 )
        postUpdate(ct1powerday, 0 )
        postUpdate(ct1powerhour,0 )
end
////////////////////////////////////////////////////////////////////////////////////////////////
rule "Energy consumption calculation"
////////////////////////////////////////////////////////////////////////////////////////////////
when
        Item ct1power received update
then
        var long currentTime = now.millis
        if (LastUpdate != 0) {
                var long timeElapsed = currentTime - LastUpdate
                if (timeElapsed > 0) {
                        var Number power = ct1power.state as DecimalType
                        var Number energyConsumption = ((power * timeElapsed) / 3600000)  / 1000 // kWh
                        postUpdate(ct1powercum, ct1powercum.state as DecimalType + energyConsumption) //increment
                        postUpdate(ct1powerday, ct1powerday.state as DecimalType + energyConsumption)
                }
        }
LastUpdate = currentTime
end
////////////////////////////////////////////////////////////////////////////////////////////////
rule "Clear daily consumption"
////////////////////////////////////////////////////////////////////////////////////////////////
when
        Time cron "0 0 0 * * ?" // every day
then
        postUpdate(ct1powerday, 0)
end
////////////////////////////////////////////////////////////////////////////////////////////////
rule "Energy in past hour"
////////////////////////////////////////////////////////////////////////////////////////////////
when
        Time cron "0 0 0/1 1/1 * ? *" // every hour
then
        postUpdate(ct1powerhour, (ct1powercum.state as DecimalType - ct1powercum.historicState(now.minusHours(1)).state as DecimalType))
end
```
## OpenHab Sitemap
```
sitemap default label="ENERGY TEST"
{
        Frame label="Energy"{
                Text item=ct1power
                Text item=ct1powerday
                Text item=ct1powerhour
        }
}
```

