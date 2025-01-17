If the performance of a Scheduler component suffers due to a high number of appointments, you can lazy load the data as necessary.

1. Modify your back-end API to accept queries with time limit parameters. For example, the query `appointments?startDate=01-01-2024&endDate=01-07-2024` should only return events within the specified time window.

2. Create a `customStore` that can pass extra arguments to your back-end code. 

    <!--- JavaScript -->

        const myCustomStore = new CustomStore({
            key: 'id',
            async load(loadOptions) => {
                const startDate = loadOptions.startDate;
                const endDate = loadOptions.endDate;
                try {
                    const response = await fetch(`https://my-server.com/api/appointments?startDate={startDate}&endDate={endDate}`);
                    const result = await response.json();
                        return {
                            data: result.data;
                        }
                } catch (err) {
                    throw new Error('Data Loading Error');
                }
            }
        });

3. To indicate that your back-end server is now responsible for filtering, set the `remoteFiltering` Scheduler option to `true`.

    ---

    ##### jQuery

        <!-- tab: index.js -->
        $("#schedulerContainer").dxScheduler({
            dataSource: myCustomStore,
            remoteFiltering: true
            ...
        });

    ##### Angular
        
        <!-- tab: app.component.html -->
        <dx-scheduler
            [dataSource]="myCustomStore"
            [remoteFiltering]="true"
            ...>
        </dx-scheduler>

    ##### Vue

        <!-- tab: App.vue -->
        <DxScheduler
            :data-source="myCustomStore"
            :remote-filtering="true"
            ...
        />

    ##### React

        <!-- tab: App.js -->
        <Scheduler
            dataSource={myCustomStore}
            remoteFiltering={true}
            ...
        />

    ---