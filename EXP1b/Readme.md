document.getElementById('reminder-form').addEventListener('submit', function(event) {
    event.preventDefault();
    
    let medicine = document.getElementById('medicine').value;
    let session = document.getElementById('session').value;
    let intake = document.getElementById('intake').value;
    let time = document.getElementById('time').value;
    let alarmTone = document.getElementById('alarm-tone').files[0];
    
    let reminderList = document.getElementById('reminder-list');
    let listItem = document.createElement('li');
    listItem.textContent = `${medicine} - ${session} (${intake}) at ${time}`;
    
    reminderList.appendChild(listItem);
    
    scheduleReminder(medicine, time, alarmTone);
});

function scheduleReminder(medicine, time, alarmTone) {
    let now = new Date();
    let reminderTime = new Date();
    let [hours, minutes] = time.split(":");
    reminderTime.setHours(hours, minutes, 0, 0);
    
    let delay = reminderTime - now;
    if (delay < 0) delay += 24 * 60 * 60 * 1000;
    
    setTimeout(() => {
        document.body.style.backgroundColor = "#ff4d4d";
        playAlarm(alarmTone);
        setTimeout(() => {
            alert(`Time to take your medicine: ${medicine}`);
        }, 1000);
    }, delay);
}



let alarmAudio;

function playAlarm(alarmTone) {
    if (alarmTone) {
        let url = URL.createObjectURL(alarmTone);
        alarmAudio = new Audio(url);
    } else {
        alarmAudio = new Audio('alarm.mp3');
    }
    alarmAudio.loop = true;
    alarmAudio.play();
}

function stopAlarm() {
    if (alarmAudio) {
        alarmAudio.pause();
        alarmAudio.currentTime = 0;
    }
}

document.addEventListener("click", stopAlarm);
