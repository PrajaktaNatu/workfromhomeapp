<template>
    <div>
        <h1>Update Item</h1>
        <div class="row">
          <div class="col-md-10"></div>
          <div class="col-md-2"><router-link :to="{ name: 'DisplayItem' }" class="btn btn-success">Return to Items</router-link></div>
        </div>

        <form v-on:submit.prevent="updateItem">
            <div class="form-group">
                <label>Name</label>
                <input type="text" class="form-control" v-model="item.name">
            </div>

            <div class="form-group">
                <label name="Reason">Reason</label>
                <input type="text" class="form-control" v-model="item.reason">
            </div>

            <div class="row">
              <div class="col-md-6">
                <div class="form-group">
                  <label>Date applying for:</label>
                  <datepicker placeholder="Select Date" v-model="item.example_date" @closed="formatDate()" :disabledDates="disabledDates"></datepicker>
                </div>
              </div>
            </div><br />

            <div class="form-group">
                <button class="btn btn-primary">Update</button>
            </div>
        </form>
    </div>
</template>

<script>

import Datepicker from "vuejs-datepicker/dist/vuejs-datepicker.esm.js";
import moment from 'moment'
import nodemailer from 'nodemailer'

    export default{

      components: {
        Datepicker
        },
        data(){
            return{
            disabledDates: {
            //    to: new Date()
            to: new Date(new Date().getTime()+(2*24*60*60*1000))
            },
            item:{}
            }
        },

        created: function(){
            this.getItem();
        },

        methods: {

        formatDate: function() {
              console.log(this.item.example_date);
              var dateString = this.item.example_date;
              var dateObj = new Date(dateString);
              var momentObj = moment(dateObj);
              this.item.example_date = momentObj.format('YYYY-MM-DD');
              console.log("Now date is: " + this.item.example_date);
        },
            getItem()
            {
              let uri = 'http://localhost:4000/items/edit/' + this.$route.params.id;
                this.axios.get(uri).then((response) => {
                    this.item = response.data;
                });
            },

            updateItem()
            {
              let uri = 'http://localhost:4000/items/update/' + this.$route.params.id;
                this.axios.post(uri, this.item).then((response) => {
                  this.$router.push({name: 'DisplayItem'});
                });
            }
        }
    }
</script>
