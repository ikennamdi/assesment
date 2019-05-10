# assesment
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" 
          content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />

    <title>Mini App</title>

    <style>
       
      div.user-photo {
        margin: 1em auto;
        width: 150px;
        height: 150px;
        border-radius: 50%;
        overflow: hidden;
      }
      
      div.select{
        margin-bottom:2.5em;
      }
      
      div.details {
        color: white;
        background: #6200ee;
        font-size: 1.3em;
        margin-top: 4em;
        padding-top: 0.5em;
        padding-bottom: 0.5em;
        padding-left: 1em;
        padding-right: 1em;
        border-radius: 10px;
      }
      
      div.details p {
        margin: 0.3em;
      }
      
      #outcome {
        position: absolute;
        right: 2.2em;
        bottom: 6.5em;
        width: 100px;
        text-align: center;
      }
      
      #outcome header {
        padding: 1em;
        background: white;
        border-radius: 10%;
        margin: none;
      }
      
      #outcome p{
        height: 40px;
        color: white;
        border-bottom:
        solid white 5px;
        font-size: 2em; 
        margin: 0; 
        padding-top: 0.5em; 
        padding-bottom: 0.5em; 
        padding-left: none; 
        padding-right: none;"
      }
      
      #oracle {
        margin-top: 2.5em;
        border: 1px solid;
        width: 100%;
      }
      
      body {
        background-color: white;
      }
      
    </style>
  </head>
  <body>
    <div id="outcome">
      <header class="mdc-typography--headline5">
        BMI
      </header>
      
      <p>
        
      </p>
    </div>
    
    <button  id="filter-query" class="mdc-icon-button material-icons">
      filter_list
    </button>
    
    <div class="select">
      <select id="select" class="select-text">
        <option selected="true" disabled="disabled">Select Option</option>
      </select>
    </div>
    
    <div class="user-photo"> 
       <img src="https://via.placeholder.com/150x150?text=Ike+Nnamdi" alt="Ike Nnamdi">
    </div>
    
    <div class="details  mdc-elevation--z3"> 
      <p>
        <span class="prop" data-age="">Age :</span>
        <span class="value" data-age-value=""> 29 years</span>
      </p>
      <p>
        <span class="prop" data-height="">Height :</span>
        <span class="value" data-height-value=""> 6 ft </span>
      </p>
      <p>
        <span class="prop" data-weight="">Weight :</span>
        <span class="value" data-weight-value=""> 78 kg </span>
      </p>
      <p>
        <span class="prop" data-gender="">Gender :</span>
        <span class="value" data-gender-value=""> Male </span>
      </p>
      <p>
        <span class="prop" data-country="">Country :</span>
        <span class="value" data-country-value=""> Nigeria </span>
      </p>
    </div>
    
    <button  id="oracle" class="mdc-button">
      Calculate BMI
    </button>
    
    <script>
      
      const users = [];
      
      const computeBMI = ({weight, height, country}) => {
        const bmiHeight = height * 0.3048;
        let calBMI = (weight/Math.pow(bmiHeight ,2));
        const healthyCountry = ["Chad","Gambia","Ghana","Israel","Ivory Coast","Mali","Senegal","Sierra Leone","Uganda"];
        if (healthyCountry.includes(country)){
          calBMI *= 0.82;
        }
        return calBMI.toFixed(1);
      };
      
      const getSelectedUser = (userId) => {
        return users.find(({id}) => id === userId);
      };
      
      const displaySelectedUser = ({target}) => {
        const user = getSelectedUser(target.value);
        const properties = Object.keys(user);
        properties.forEach(prop => {
          const span = document.querySelector(`span[data-${prop}-value]`);
          if(span) {
            span.textContent= user[prop];
          }
        })
      }
      
      const letsCalculateBMI = () => {
        const user = getSelectedUser(selectText.value);
        let bmi = computeBMI(user);
        document.querySelector("#outcome p").textContent = bmi;
      }
      
      const powerupTheUI = () => {        
        document.querySelector(".select-text").addEventListener("change", displaySelectedUser);
                                   
        document.querySelector("#oracle").addEventListener("click", letsCalculateBMI);
      };
      
      

      const displayUsers = (users) => {
        users.forEach(user => {
            const select = document.querySelector('.select-text');
            const option = document.createElement('option');
			
   
            option.innerHTML = user.name; 
            option.value = user.id;
            select.appendChild(option);
        });
      };
      
      const fetchAndDisplayUsers = () => {
        users.push({
          age: 40,
          weight: 75,
          height: 6,
          country: 'Nigeria',
          name: 'Charles Odili',
          id: 'dfhb454768DghtF'
        },
        {
          age: 24,
          weight: 73,
          height: 5.7,
          country: 'Nigeria',
          name: 'Azih Chimezie',
          id: 'gahThaj8e7'
        },
        {
          age: 26,
          weight: 78,
          height: 6.1,
          country: 'Nigeria',
          name: 'Ike Nnamdi',
          id: 'hfd34598GgfsT'
        });
        
        displayUsers(users);
        
        // api declartion
        const api = 'https://randomapi.com/api/y1lfp11q?key=LEIX-GF3O-AG7I-6J84';
        //fetch API to make a HTTP call
        fetch(api)
        //convert response to JSON
          .then(response => response.json())
          .then(({ results }) => {
          let [user] = results;
          
          users.push(user);
          displayUsers([...users, user])

        })
        .catch(error => console.log(error))
      };
      
      const startApp = () => {
        powerupTheUI();
        fetchAndDisplayUsers();
      };
      
      startApp();
      
    </script>
  </body>
</html>
