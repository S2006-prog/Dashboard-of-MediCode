# Dashboard-of-MediCode
All about Medicode
import React, { useState, useEffect } from "react";
import { MedicalCode, Patient, CodeMapping } from "@/entities/all";
import { 
  Activity, 
  Users, 
  Stethoscope, 
  BarChart3,
  TrendingUp,
  Clock,
  Shield,
  Database
} from "lucide-react";

import StatsCard from "../components/dashboard/StatsCard";
import RecentActivity from "../components/dashboard/RecentActivity";
import SystemHealth from "../components/dashboard/SystemHealth";

export default function Dashboard() {
  const [stats, setStats] = useState({
    totalCodes: 0,
    totalPatients: 0,
    totalMappings: 0,
    recentActivity: 0
  });
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    loadDashboardData();
  }, []);

  const loadDashboardData = async () => {
    setIsLoading(true);
    try {
      const [codes, patients, mappings] = await Promise.all([
        MedicalCode.list(),
        Patient.list(),
        CodeMapping.list()
      ]);
      
      setStats({
        totalCodes: codes.length,
        totalPatients: patients.length,
        totalMappings: mappings.length,
        recentActivity: 24
      });
    } catch (error) {
      console.error("Error loading dashboard data:", error);
    }
    setIsLoading(false);
  };

  return (
    <div className="space-y-8">
      {/* Header */}
      <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
        <div>
          <h1 className="text-3xl font-bold text-gray-800 mb-2">Dashboard</h1>
          <p className="text-gray-600">NAMASTE & ICD-11 TM2 Integration Platform</p>
        </div>
        <div className="neumorphic-button px-6 py-3 rounded-xl">
          <div className="flex items-center gap-2 text-gray-700">
            <Shield className="w-4 h-4" />
            <span className="font-medium">FHIR R4 Compliant</span>
          </div>
        </div>
      </div>

      {/* Stats Grid */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <StatsCard
          title="Medical Codes"
          value={stats.totalCodes}
          icon={Database}
          subtitle="NAMASTE + ICD-11"
          isLoading={isLoading}
        />
        <StatsCard
          title="Patient Records"
          value={stats.totalPatients}
          icon={Users}
          subtitle="With dual coding"
          isLoading={isLoading}
        />
        <StatsCard
          title="Code Mappings"
          value={stats.totalMappings}
          icon={Stethoscope}
          subtitle="Verified mappings"
          isLoading={isLoading}
        />
        <StatsCard
          title="Recent Activity"
          value={stats.recentActivity}
          icon={Activity}
          subtitle="Last 24 hours"
          isLoading={isLoading}
        />
      </div>

      {/* Main Content Grid */}
      <div className="grid lg:grid-cols-3 gap-8">
        <div className="lg:col-span-2">
          <RecentActivity />
        </div>
        <div>
          <SystemHealth />
        </div>
      </div>

      {/* Quick Actions */}
      <div className="neumorphic-card p-6">
        <h3 className="text-xl font-bold text-gray-800 mb-6">Quick Actions</h3>
        <div className="grid md:grid-cols-3 gap-4">
          <button className="neumorphic-button p-4 rounded-xl text-left">
            <Database className="w-8 h-8 text-blue-600 mb-2" />
            <h4 className="font-semibold text-gray-800">Import Codes</h4>
            <p className="text-sm text-gray-600">Add new NAMASTE or ICD-11 codes</p>
          </button>
          <button className="neumorphic-button p-4 rounded-xl text-left">
            <Users className="w-8 h-8 text-green-600 mb-2" />
            <h4 className="font-semibold text-gray-800">New Patient</h4>
            <p className="text-sm text-gray-600">Register patient with dual coding</p>
          </button>
          <button className="neumorphic-button p-4 rounded-xl text-left">
            <Stethoscope className="w-8 h-8 text-purple-600 mb-2" />
            <h4 className="font-semibold text-gray-800">Map Codes</h4>
            <p className="text-sm text-gray-600">Create new terminology mappings</p>
          </button>
        </div>
      </div>
    </div>
  );
}
