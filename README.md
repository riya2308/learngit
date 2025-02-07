<div class="modal">
    <div class="modal-header">
        <div class="header-content">
            <img src="powershell-icon.png" alt="PowerShell Icon" class="powershell-icon">
            <div>
                <div class="modal-title">Windows PowerShell (x86)</div>  
            </div>
        </div>
        <button type="button" class="close" onclick="onClose()">&times;</button>
    </div>
    <div class="modal-body">
        <h2>
            Workforce Technology
                <div class="modal-subtitle">02/05/2025 17:10:11</div>
                </h2>
        <h2>The title should be given</h2>
        <p>The body for the TestingPopup5thFebTc01 is not empty</p>
        <div class="dropdown">
            <label for="read-later-interval">Select a Read Later Interval</label>
            <select id="read-later-interval" class="select-dropdown">
                <option>15 Minutes</option>
                <option>30 Minutes</option>
                <option>1 Hour</option>
            </select>
        </div>
        <div class="footer-buttons">
            <button class="btn-primary">Learn More</button>
            <button class="btn-default">Read Later</button>
            <button class="btn-default">Close</button>
        </div>
    </div>
</div>







css:

.modal {
    background-color: black;
    color: white;
    border-radius: 10px;
    width: 600px; /* Adjust based on your layout needs */
    min-height: 350px; /* Adjust height as needed */
    padding: 20px;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 5px; /* Reduced bottom padding */
}

.header-content {
    display: flex;
    align-items: center;
}

.powershell-icon {
    width: 30px;
    height: 30px;
    margin-right: 10px;
}

.modal-title {
    font-size: 16px;
    margin: 0;
    line-height: 1.25; /* Adjust line height for tighter spacing */
}

.modal-subtitle {
    font-size: 12px;
    color: #888;
}

.close {
    cursor: pointer;
    border: none;
    background: none;
    color: white;
    font-size: 24px;
}

.modal-body {
    padding-top: 5px; /* Reduced top padding */
}

.select-dropdown {
    width: 100%;
    color: white; /* Text color */
    background-color: #333; /* Dropdown background */
    padding: 5px;
    border-radius: 5px;
    border: 1px solid #555; /* Defines the dropdown clearly */
}

.footer-buttons {
    margin-top: 20px; /* Ensures spacing above buttons */
}

.footer-buttons button {
    margin-right: 10px;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #333;
    color: white;
    cursor: pointer;
    font-size: 14px;
}

.btn-primary {
    background-color: #007bff;
}

.btn-default {
    background-color: #6c757d;
}
