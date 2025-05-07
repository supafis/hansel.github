# hansel.github
% Set folder path
folder = 'C:\Users\User\Desktop\MM466\Project\Attempt_post_review\ALL DATA';
fileList = dir(fullfile(folder, '*.mat'));

% Initialize
allVibrations = [];
fileNames = {};  % Will hold column names

for k = 1:length(fileList)
    % Full path
    filename = fullfile(folder, fileList(k).name);
    
    % Load the .mat file
    fileData = load(filename);
    
    % Extract Channel 1 (assume it's the first variable or known name)
    varNames = fieldnames(fileData);
    channel1 = fileData.(varNames{1});  % Adjust if needed
    
    % Concatenate horizontally
    allVibrations = [allVibrations, channel1];
    
    % Clean file name (remove extension, ensure it's a valid variable name)
    [~, nameOnly, ~] = fileparts(fileList(k).name);
    validName = matlab.lang.makeValidName(nameOnly);  % Ensures valid table column names
    fileNames{end+1} = validName;
end

% Convert to table
VibTable = array2table(allVibrations, 'VariableNames', fileNames);

% Save the table to .mat file
save(fullfile(folder, 'Vibtable.mat'), 'VibTable');

% save the filename table
save(fullfile(folder, 'all_variables.mat'));

%if the above code has already been run, load 'vibtable' from the current folder
load all_variables.mat 

A simple Summary of the combined table of all frequencies
summary(VibTable) 
Healthy bearings have the lowest median indicating that it has the smoothest frequency. This is expected as it is supposed to smooth rotation.
Inner Race defect has the highest median. This was unnexpected as it was thought that as the location of the defect approaches the center, the vibration deviation would be smaller. This was not the case here. There was a consistent outlier for one test run.
Scratched ball defect presents itself as the third most stable frequency and this is present in its location within the bearing. Since it is the object with the highest contact, it clearly separates itself from the first two classes.
Outer ring defect has noticeable deviations. It has a large median as it is the furtherest away from the center of the bearing. Any defects present significantly affect the smoothness of rotation. This class theoretically should be easier to distinguish from others.
Combination of defects varies between high and low variances. This may be difficult to categorise later on.

figure('Position', [100, 100, 1000, 600]);  
hold on;
for k = 1:width(VibTable)
    plot(VibTable{1:10000, k}, 'DisplayName', fileNames{k});
end
hold off;
xlabel('Sample Index');
ylabel('Acceleration (m/s^2');
title('Raw Vibration Points (First 10,000 Samples)');

% Two-column legend at the bottom
legend('show', 'Location', 'eastoutside', 'NumColumns', 2);
set(gca, 'Position', [0.1, 0.2, 0.7, 0.7]);  % Resize axes to make room
grid on;

The above Graph shows the frequencies for all the 5 classes across their 3 trials of 4 different rotational speeds. The graph was only captured for the first 10,000 samples.

Upon further inspection, it can be seen that the 'Healthy' class have very little frequency deviations.

Inner race clearly shows large deviations from the center. This was explained earlier in the summary. This visualisation shows that most outliers are due to inner race defect.


figure('Position', [100, 100, 1000, 600]);  
hold on;
for k = 25:35
    plot(VibTable{1:10000, k}, 'DisplayName', fileNames{k});
end
hold off;
xlabel('Sample Index');
ylabel('Acceleration (m/s^2');
title('Raw Vibration points of Healthy Bearing (First 10,000 Samples)');

% Two-column legend at the bottom
legend('show', 'Location', 'eastoutside', 'NumColumns', 2);
set(gca, 'Position', [0.1, 0.2, 0.7, 0.7]);  % Resize axes to make room
grid on;

The healthy bearing vibration samples.

figure('Position', [100, 100, 1000, 600]);  
hold on;
for k = 1:12
    plot(VibTable{1:10000, k}, 'DisplayName', fileNames{k});
end
hold off;
xlabel('Sample Index');
ylabel('Acceleration (m/s^2');
title('Raw Vibration points of Scratched Ball Defect(First 10,000 Samples)');

% Two-column legend at the bottom
legend('show', 'Location', 'eastoutside', 'NumColumns', 2);
set(gca, 'Position', [0.1, 0.2, 0.7, 0.7]);  % Resize axes to make room
grid on;


The vibration points for Scratched Ball defect are quite linear except for BA3, which had a lot of outliers.

figure('Position', [100, 100, 1000, 600]);  
hold on;
for k = 37:48
    plot(VibTable{1:10000, k}, 'DisplayName', fileNames{k});
end
hold off;
xlabel('Sample Index');
ylabel('Acceleration (m/s^2');
title('Raw Vibration points of Inner Ring Defect(First 10,000 Samples)');

% Two-column legend at the bottom
legend('show', 'Location', 'eastoutside', 'NumColumns', 2);
set(gca, 'Position', [0.1, 0.2, 0.7, 0.7]);  % Resize axes to make room
grid on;

The vibration for the inner ring largely differs. There was one sample which consistently had an increased acceleration when tested.

figure('Position', [100, 100, 1000, 600]);  
hold on;
for k = 49:60
    plot(VibTable{1:10000, k}, 'DisplayName', fileNames{k});
end
hold off;
xlabel('Sample Index');
ylabel('Acceleration (m/s^2');
title('Raw Vibration points of Outer Ring Defect(First 10,000 Samples)');

% Two-column legend at the bottom
legend('show', 'Location', 'eastoutside', 'NumColumns', 2);
set(gca, 'Position', [0.1, 0.2, 0.7, 0.7]);  % Resize axes to make room
grid on;
