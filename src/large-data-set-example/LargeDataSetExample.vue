<template>
    <div style="width: 100%;">
        <h1>Large Data Set Component ({{rownum | localeNum}} rows)</h1>
        <ag-grid-vue style="width: 100%; height: 650px;" class="ag-theme-alpine"
                     :rowData="rowData"
                     :columnDefs="columnDefs"
                     :modules="modules"
					 rowSelection="multiple"
                     @grid-ready="onReady">
        </ag-grid-vue>
    </div>
</template>

<script>
    import {AgGridVue} from "@ag-grid-community/vue";
    // for community features
    import {AllCommunityModules} from "@ag-grid-community/all-modules";

    // for enterprise features
    // import {AllModules} from "@ag-grid-enterprise/all-modules";

    export default {
        name: "LargeDataSetExample",
        data() {
            return {
                rowData: this.rowData,
                columnDefs: this.columnDefs,
                modules: AllCommunityModules,
				rownum : ""
            }
        },
        components: {
            'ag-grid-vue': AgGridVue
        },
        created() {
            // data created here so outside of vue (ie no reactive, not observed)
            // also frozen (prob unnecessarily)
            this.rowData = [];
			
			let rn = 0;
            
			for (let i = 0; i < 100000; i++) {
				
				rn ++;
                
				this.rowData.push(Object.freeze({
                    recordNumber: rn,
                    value1: Math.floor(Math.random() * 10000),
                    value2: Math.floor(Math.random() * 10000),
                    value3: Math.floor(Math.random() * 10000),
                    value4: Math.floor(Math.random() * 10000),
                    value5: Math.floor(Math.random() * 10000),
                    value6: Math.floor(Math.random() * 10000),
                    value7: Math.floor(Math.random() * 10000)
                }));
            }
			
			this.rownum = rn;
			
            this.rowData = Object.freeze(this.rowData);

            this.columnDefs = Object.freeze([
				{
                   headerName: '#', minWidth: 60, width: 60, checkboxSelection: true, sortable: false,
                   suppressMenu: true, pinned: true
                },
				{headerName: 'Record', field: 'recordNumber'},
                {headerName: 'Value 1', field: 'value1'},
                {headerName: 'Value 2', field: 'value2'},
                {headerName: 'Value 3', field: 'value3'},
                {headerName: 'Value 4', field: 'value4'},
                {headerName: 'Value 5', field: 'value5'},
                {headerName: 'Value 6', field: 'value6'},
                {headerName: 'Value 7', field: 'value7'}
            ])
        },
        methods: {
            onReady(params) {				
                console.log(params.api.sizeColumnsToFit());
            }
        },
		filters : {
			localeNum : function(val){
				return val.toLocaleString()
			}
		}
    }
</script>

<style>
</style>
