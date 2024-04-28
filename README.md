public columnDefs: ColDef[] = [
    { headerName: 'Notify Email Group', field: 'notifyEmailGroup' },
    { headerName: 'Share Name', field: 'shareName' },
    { headerName: 'Usage', field: 'usage', type: 'rightAligned' }, // Assuming this is a numeric field
    { headerName: 'Final Increase Value (GB)', field: 'finalIncreaseValue', type: 'rightAligned' }, // Assuming this is a numeric field
    { headerName: 'Path', field: 'path' },
    { headerName: 'Days to Pserve', field: 'daysToPserve', type: 'rightAligned' }, // Assuming this is a numeric field
    { headerName: 'Pserver', field: 'pserver' },
    { headerName: 'Division', field: 'division' },
    { headerName: 'PServer + Share Name', field: 'pserverPlusShareName' }
];

public rowData: any[] = [
    {
        notifyEmailGroup: 'ibd-nasquota-amer',
        shareName: 'stor186/s195685',
        usage: 89.05, // Example format for numeric data
        finalIncreaseValue: 268.18,
        path: 'Some Path',
        daysToPserve: -1,
        pserver: 'stor186ncs1.new-york.ms.com',
        division: 'ibd',
        pserverPlusShareName: 'Combination or Calculated Field'
    },
    // ... more rows based on your data
];
