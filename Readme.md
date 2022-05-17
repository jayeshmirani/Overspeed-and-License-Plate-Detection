# Vehicle Overspeed Detection and Automatic License Plate Recognition

#### Weights
- Weights/best.pt : for Vehicle Detection
- Weights/bestLP.pt : for License Plate Detection
- Weights/roboflow_last.pt : for License Plate Recognition

# Code

#### In command prompt run the following code (which runs track.py file):
C:\Users\Mobile Workstation\Desktop\Overspeed and License Plate Detection\Yolov5_DeepSort_Pytorch>python track.py --source wardha2.mp4 --yolo_model best.pt --show-vid

## Vehicle Detection and Tracking (track.py file)
- Runs YOLOv5 (Vehicle Detection) + DeepSORT (Vehicle Tracking) models (take and save 10 snapshots of each vehicle)
- User should edit Physical distance according to road length of video (variable: phys_dis)
- Set flag lines (vars.: Flag1 (start_point1, end_point1), Flag2 (start_point2, end_point2))

At end of track.py, following code is present, which creates a asynchronous process through a new system call (goes to tempLP.py):

cmd_lp = 'python3 ./yolov5/tempLP.py ' + dir_path + ' ' + str(output[4])  + ' ' + str(cars_dic[output[4]][3])  + ' ' + str(cars_dic[output[4]][4])  + ' ' + str(cars_dic[output[4]][5]) + ' ' + str(cars_dic[output[4]][7]) + ' ' + str(next_entry_index)
os.system(f'start cmd /c {cmd_lp}')

## License Plate Detection
#### tempLP.py
- YOLOv5: Extracts license plate from snapshots of vehicle
- Save 10 snapshots of license plates for each vehicle

At the end of tempLP.py file, for each license plate snapshot, the following code is ran(which calls tempOCR.py) and we get the correct license plate:
correct_license_plate_no =  runOCR(source=dir_path_label, weights="./yolov5/roboflow_last.pt")

- Add vehicle start frame, end frame, speed and license plate number in excel sheet

## License Plate Recognition
#### tempOCR.py
- Apply Optical-Character-Recognition on extracted license plate
- YOLOv5 model: for detection of 36 classes; 26 characters from A-Z and 10 digits from 0-9. 
- return license plate no. to tempLP
