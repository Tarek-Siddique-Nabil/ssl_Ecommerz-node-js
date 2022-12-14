# ssl_Ecommerz-node-js
const express = require('express')
const app = express()

const SSLCommerzPayment = require('sslcommerz-lts')
const store_id = 'bijoy63971e20806b0'
const store_passwd = 'bijoy63971e20806b0@ssl'
const is_live = true //true for live, false for sandbox

const port = 3030

//sslcommerz init
app.get('/init', (req, res) => {
    const data = {
        total_amount: 100,
        currency: 'BDT',
        tran_id: Math.floor(Math.random()*16777215954555625291).toString(16), // use unique tran_id for each api call
        success_url: 'http://localhost:3030//ssl-payment-success',
        fail_url: 'http://localhost:3030/ssl-payment-fail',
        cancel_url: 'http://localhost:3030/ssl-payment-cancel',
        shipping_method: 'Courier',
        product_name: 'Computer.',
        product_category: 'Electronic',
        product_profile: 'general',
        cus_name: 'Customer Name',
        cus_email: 'customer@example.com',
        cus_add1: 'Dhaka',
        cus_add2: 'Dhaka',
        cus_city: 'Dhaka',
        cus_state: 'Dhaka',
        cus_postcode: '1000',
        cus_country: 'Bangladesh',
        cus_phone: '01711111111',
        cus_fax: '01711111111',
        ship_name: 'Customer Name',
        ship_add1: 'Dhaka',
        ship_add2: 'Dhaka',
        ship_city: 'Dhaka',
        ship_state: 'Dhaka',
        ship_postcode: 1000,
        ship_country: 'Bangladesh',
        ipn_url: 'http://localhost:3030/ssl-payment-notification',
    };
    const sslcommerz = new SSLCommerzPayment(store_id, store_passwd, is_live) //true for live default false for sandbox
    sslcommerz.init(data).then(data => {
  
      //process the response that got from sslcommerz 
      //https://developer.sslcommerz.com/doc/v4/#returned-parameters
  
      if (data?.GatewayPageURL) {
        return res.status(200).redirect(data?.GatewayPageURL);
      }
      else {
        return res.status(400).json({
          message: "Session was not successful"
        });
      }
    });
  
  });
  
  app.post("/ssl-payment-notification", async (req, res) => {
  
    /** 
    * If payment notification
    */
  
    return res.status(200).json(
      {
        data: req.body,
        message: 'Payment notification'
      }
    );
  })
  
  app.post("/ssl-payment-success", async (req, res) => {
  
    /** 
    * If payment successful 
    */
  
    return res.status(200).json(
      {
        data: req.body,
        message: 'Payment success'
      }
    );
  })
  
  app.post("/ssl-payment-fail", async (req, res) => {
  
    /** 
    * If payment failed 
    */
  
    return res.status(200).json(
      {
        data: req.body,
        message: 'Payment failed'
      }
    );
  })
  
  app.post("/ssl-payment-cancel", async (req, res) => {
  
    /** 
    * If payment cancelled 
    */
  
    return res.status(200).json(
      {
        data: req.body,
        message: 'Payment cancelled'
      }
    );
  })

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`)
})
