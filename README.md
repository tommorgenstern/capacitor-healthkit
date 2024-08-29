# Capacitor HealthKit Plugin

:heart: Capacitor plugin to retrieve data from HealthKit :heart:

Disclaimer : _for now only some of the HK data base, in the future the retrieve base will be bigger !_

## Getting Started

### Prerequisites

* Add **HealthKit to your Xcode project** (section signing & capabilities)

![alt text](https://i.ibb.co/Bg03ZKf/auth-hk.png)

* ADD **Privacy - Health Share Usage Description** to your Xcode project
* ADD **Privacy - Health Update Usage Description** to your Xcode project

You can simply put this into the `info.plist` file

```
	<key>NSHealthShareUsageDescription</key>
	<string>Read Health Data</string>
	<key>NSHealthUpdateUsageDescription</key>
	<string>Read Health Data</string>
```

### Installing

Do

```
npm i --save @perfood/capacitor-healthkit
```

Then

```
npx cap update
```

And  **if you use Ionic or Angular, here a example setup:**

in your .ts file add this:

```
import {
  ActivityData,
  CapacitorHealthkit,
  OtherData,
  QueryOutput,
  SampleNames,
  SleepData,
} from '@perfood/capacitor-healthkit';

const READ_PERMISSIONS = ['calories', 'stairs', 'activity', 'steps', 'distance', 'duration', 'weight'];

```

and then you can create async functions like this:

```


  public async requestAuthorization(): Promise<void> {
    try {
      await CapacitorHealthkit.requestAuthorization({
        all: [''],
        read: READ_PERMISSIONS,
        write: [''],
      });

    } catch (error) {
      console.error('[HealthKitService] Error getting Authorization:', error);
    }
  }

  private async getActivityData(
    startDate: Date,
    endDate: Date = new Date(),
  ): Promise<QueryOutput<ActivityData>> | undefined {
    try {
      const queryOptions = {
        sampleName: SampleNames.WORKOUT_TYPE,
        startDate: startDate.toISOString(),
        endDate: endDate.toISOString(),
        limit: 0,
      };

      return await CapacitorHealthkit.queryHKitSampleType<ActivityData>(queryOptions);
    } catch (error) {
      console.error(error);

      return undefined;
    }
  }
```

so you can use the plugin for example with the call `CapacitorHealthkit.queryHKitSampleType(...`

And you're all set ! :+1:

<docgen-index>

* [`requestAuthorization(...)`](#requestauthorization)
* [`queryHKitSampleType(...)`](#queryhkitsampletype)
* [`isAvailable()`](#isavailable)
* [`multipleQueryHKitSampleType(...)`](#multiplequeryhkitsampletype)
* [`isEditionAuthorized(...)`](#iseditionauthorized)
* [`multipleIsEditionAuthorized(...)`](#multipleiseditionauthorized)
* [`queryHKitStatisticsCollection(...)`](#queryhkitstatisticscollection)
* [`queryHKitStatistics(...)`](#queryhkitstatistics)
* [Interfaces](#interfaces)

</docgen-index>

<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### requestAuthorization(...)

```typescript
requestAuthorization(authOptions: AuthorizationQueryOptions) => Promise<void>
```

This functions will open the iOS Screen to let users choose their permissions. Keep in mind as developers, if the access has been denied by the user we will have no way of knowing - the query results will instead just be empty arrays.

| Param             | Type                                                                            | Description                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **`authOptions`** | <code><a href="#authorizationqueryoptions">AuthorizationQueryOptions</a></code> | These define which access we need. Possible Options include ['calories', 'stairs', 'activity', 'steps', 'distance', 'duration', 'weight']. |

--------------------


### queryHKitSampleType(...)

```typescript
queryHKitSampleType<T>(queryOptions: SingleQueryOptions) => Promise<QueryOutput<T>>
```

This defines a query to the Healthkit for a single type of data.

| Param              | Type                                                              | Description                                                                                                            |
| ------------------ | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **`queryOptions`** | <code><a href="#singlequeryoptions">SingleQueryOptions</a></code> | defines the type of data and the timeframe which shall be queried, a limit can be set to reduce the number of results. |

**Returns:** <code>Promise&lt;<a href="#queryoutput">QueryOutput</a>&lt;T&gt;&gt;</code>

--------------------


### isAvailable()

```typescript
isAvailable() => Promise<void>
```

This functions resolves if HealthKitData is available it uses the native HKHealthStore.isHealthDataAvailable() function of the HealthKit.

--------------------


### multipleQueryHKitSampleType(...)

```typescript
multipleQueryHKitSampleType(queryOptions: MultipleQueryOptions) => Promise<any>
```

This defines a query to the Healthkit for multiple types of data. This function has not been tested.

| Param              | Type                                                                  | Description                                       |
| ------------------ | --------------------------------------------------------------------- | ------------------------------------------------- |
| **`queryOptions`** | <code><a href="#multiplequeryoptions">MultipleQueryOptions</a></code> | defines the sample types which can be queried for |

**Returns:** <code>Promise&lt;any&gt;</code>

--------------------


### isEditionAuthorized(...)

```typescript
isEditionAuthorized(queryOptions: EditionQuery) => Promise<void>
```

Checks if there is writing permission for one specific sample type. This function has not been tested.

| Param              | Type                                                  | Description                                                                 |
| ------------------ | ----------------------------------------------------- | --------------------------------------------------------------------------- |
| **`queryOptions`** | <code><a href="#editionquery">EditionQuery</a></code> | defines the sample type for which you need to check for writing permission. |

--------------------


### multipleIsEditionAuthorized(...)

```typescript
multipleIsEditionAuthorized(queryOptions: MultipleEditionQuery) => Promise<void>
```

Checks if there is writing permission for multiple sample types. This function has not been tested.

| Param              | Type                                                                  | Description                                                                  |
| ------------------ | --------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **`queryOptions`** | <code><a href="#multipleeditionquery">MultipleEditionQuery</a></code> | defines the sample types for which you need to check for writing permission. |

--------------------


### queryHKitStatisticsCollection(...)

```typescript
queryHKitStatisticsCollection(queryOptions: StatisticsCollectionQueryOptions) => Promise<QueryOutput<AggregatedData>>
```

This defines a query to HealthKit for aggregated data over a specific time interval using HKStatisticsCollectionQuery.

| Param              | Type                                                                                          | Description                                                        |
| ------------------ | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **`queryOptions`** | <code><a href="#statisticscollectionqueryoptions">StatisticsCollectionQueryOptions</a></code> | defines the type of data, timeframe, and interval for aggregation. |

**Returns:** <code>Promise&lt;<a href="#queryoutput">QueryOutput</a>&lt;<a href="#aggregateddata">AggregatedData</a>&gt;&gt;</code>

--------------------


### queryHKitStatistics(...)

```typescript
queryHKitStatistics(queryOptions: StatisticsQueryOptions) => Promise<QueryOutput<AggregatedData>>
```

This defines a query to HealthKit for statistical data using HKStatisticsQuery.

| Param              | Type                                                                      | Description                                                                  |
| ------------------ | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **`queryOptions`** | <code><a href="#statisticsqueryoptions">StatisticsQueryOptions</a></code> | defines the type of data, timeframe, and statistics options for aggregation. |

**Returns:** <code>Promise&lt;<a href="#queryoutput">QueryOutput</a>&lt;<a href="#aggregateddata">AggregatedData</a>&gt;&gt;</code>

--------------------


### Interfaces


#### AuthorizationQueryOptions

Used for authorization of reading and writing access.

| Prop        | Type                  |
| ----------- | --------------------- |
| **`read`**  | <code>string[]</code> |
| **`write`** | <code>string[]</code> |
| **`all`**   | <code>string[]</code> |


#### QueryOutput

This interface is used for any results coming from HealthKit. It always has a count and the actual results.

| Prop              | Type                |
| ----------------- | ------------------- |
| **`countReturn`** | <code>number</code> |
| **`resultData`**  | <code>T[]</code>    |


#### SingleQueryOptions

This extends the Basequeryoptions for a single sample type.

| Prop             | Type                |
| ---------------- | ------------------- |
| **`sampleName`** | <code>string</code> |


#### MultipleQueryOptions

This extends the Basequeryoptions for a multiple sample types.

| Prop              | Type                  |
| ----------------- | --------------------- |
| **`sampleNames`** | <code>string[]</code> |


#### EditionQuery

This is used for checking writing permissions.

| Prop             | Type                |
| ---------------- | ------------------- |
| **`sampleName`** | <code>string</code> |


#### MultipleEditionQuery

This is used for checking writing permissions.

| Prop              | Type                  |
| ----------------- | --------------------- |
| **`sampleNames`** | <code>string[]</code> |


#### AggregatedData

Data structure for aggregated data, such as step counts aggregated over a period of time.

| Prop            | Type                |
| --------------- | ------------------- |
| **`startDate`** | <code>string</code> |
| **`endDate`**   | <code>string</code> |
| **`value`**     | <code>number</code> |


#### StatisticsCollectionQueryOptions

These query options define the parameters for retrieving aggregated data using HKStatisticsCollectionQuery.

| Prop               | Type                                     |
| ------------------ | ---------------------------------------- |
| **`sampleName`**   | <code>string</code>                      |
| **`startDate`**    | <code>string</code>                      |
| **`endDate`**      | <code>string</code>                      |
| **`intervalUnit`** | <code>'day' \| 'hour' \| 'minute'</code> |
| **`unit`**         | <code>string</code>                      |


#### StatisticsQueryOptions

These query options define the parameters for retrieving statistical data using HKStatisticsQuery.

| Prop                    | Type                                                                                                        |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| **`sampleName`**        | <code>string</code>                                                                                         |
| **`startDate`**         | <code>string</code>                                                                                         |
| **`endDate`**           | <code>string</code>                                                                                         |
| **`statisticsOptions`** | <code>'cumulativeSum' \| 'discreteAverage' \| 'discreteMax' \| 'discreteMin' \| 'discreteMostRecent'</code> |
| **`unit`**              | <code>string</code>                                                                                         |

</docgen-api>


## Credits

* Theo Creach (original author) - [Twitter](https://twitter.com/crcht)
* Timothée Bilodeau - [Linkedin](https://www.linkedin.com/in/timoth%E9e-bilodeau-03080ab2/)

## License

This project is licensed under the MIT License
