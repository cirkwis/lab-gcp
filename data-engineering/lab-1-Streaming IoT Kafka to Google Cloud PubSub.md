# Overview
In this lab, you launch a Confluent Kafka instance and use it to communicate with Cloud Pub/Sub. You learn how to:

- Initiate Cloud Launcher to create an instance of Confluent Kafka
- Configure a Kafka connector to integrate with Pub/Sub
- Setup topics and subscriptions for message communication
- Perform basic testing of both Kafka and Cloud Pub/Sub services
- Connect IoT Core to Cloud Pub/Sub

Source: [Qwicklab](https://www.qwiklabs.com/focuses/2766?parent=catalog)

For this lab you will be using multiple SSH sessions.You might want to think about how to organize them on your desktop. Leave these windows open during your lab.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-1wig{font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">Name</th>
    <th class="tg-7btt">Identifier</th>
    <th class="tg-1wig">Description</th>
  </tr>
  <tr>
    <td class="tg-0pky">Kafka Instance</td>
    <td class="tg-0pky">SSH Window A</td>
    <td class="tg-0lax">This window will be used to run the application in the backgroud</td>
  </tr>
  <tr>
    <td class="tg-0pky">Kafka Console</td>
    <td class="tg-0pky">SSH Window B<br>(Publisher/Consumer)</td>
    <td class="tg-0lax">This window is used to access the Kafka command line</td>
  </tr>
  <tr>
    <td class="tg-0pky">Cloud PubSub Console</td>
    <td class="tg-0pky">Cloud Shell C.</td>
    <td class="tg-0lax">This shell is required to setup PubSub and view the queue</td>
  </tr>
</table>

![SSH sessions](./image/lab-1-1.png "SSH sessions")

